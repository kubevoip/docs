---
title: "Kubernetes VoIP quickstart"
description: "Create a KubeVoIP SIP platform with two phones that can call each other through Kamailio and RTPengine on Kubernetes."
url: "/getting-started/quickstart/"
---

# Kubernetes VoIP quickstart

This quickstart creates a small SIP on Kubernetes deployment with two users:
`alice` on extension `100` and `bob` on extension `101`. Helm installs the
operator, a short inline manifest creates the temporary database and base
platform, and the `kubevoip` CLI creates the SIP users and routes.

```bash
helm install kubevoip oci://ghcr.io/kubevoip/charts/kubevoip \
  --version 0.5.0 \
  --namespace telephony --create-namespace

kubectl apply -f - <<'YAML'
apiVersion: v1
kind: Secret
metadata:
  name: postgres-app
  namespace: telephony
type: Opaque
stringData:
  host: postgres
  port: "5432"
  dbname: kubevoip
  user: kubevoip
  password: kubevoip-demo-password
---
apiVersion: v1
kind: Service
metadata:
  name: postgres
  namespace: telephony
spec:
  selector:
    app: kubevoip-demo-postgres
  ports:
    - port: 5432
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
  namespace: telephony
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kubevoip-demo-postgres
  template:
    metadata:
      labels:
        app: kubevoip-demo-postgres
    spec:
      containers:
        - name: postgres
          image: postgres:16-alpine
          env:
            - name: POSTGRES_DB
              value: kubevoip
            - name: POSTGRES_USER
              value: kubevoip
            - name: POSTGRES_PASSWORD
              value: kubevoip-demo-password
          ports:
            - containerPort: 5432
---
apiVersion: kubevoip.com/v1alpha1
kind: NetworkProfile
metadata:
  name: public
  namespace: telephony
spec:
  externalAddress:
    source: Service
  localNetworks:
    - 10.0.0.0/8
---
apiVersion: kubevoip.com/v1alpha1
kind: MediaRelay
metadata:
  name: main
  namespace: telephony
spec:
  replicas: 1
  networkProfileRef:
    name: public
  media:
    start: 20000
    end: 20049
  network:
    mode: Service
    service:
      type: LoadBalancer
      externalTrafficPolicy: Local
---
apiVersion: kubevoip.com/v1alpha1
kind: SIPGateway
metadata:
  name: main
  namespace: telephony
spec:
  replicas: 1
  databaseSecretRef:
    name: postgres-app
  networkProfileRef:
    name: public
  mediaRelayRef:
    name: main
  service:
    type: LoadBalancer
    externalTrafficPolicy: Local
YAML

printf '%s' 'alice-demo-password' | uvx kubevoip --schema-source cluster -n telephony secret sip-user alice-sip --from-stdin
printf '%s' 'bob-demo-password' | uvx kubevoip --schema-source cluster -n telephony secret sip-user bob-sip --from-stdin
uvx kubevoip --schema-source cluster -n telephony scope create internal --gateway main
uvx kubevoip --schema-source cluster -n telephony policy create internal-only --gateway main --scope internal
uvx kubevoip --schema-source cluster -n telephony user create alice --extension 100 --gateway main --dial-policy internal-only --auth-username alice --caller-id "Alice <100>" --password-secret alice-sip
uvx kubevoip --schema-source cluster -n telephony user create bob --extension 101 --gateway main --dial-policy internal-only --auth-username bob --caller-id "Bob <101>" --password-secret bob-sip
uvx kubevoip --schema-source cluster -n telephony route create user-100 --gateway main --scope internal --priority 100 --match 100 --target-user alice
uvx kubevoip --schema-source cluster -n telephony route create user-101 --gateway main --scope internal --priority 100 --match 101 --target-user bob

kubectl -n telephony rollout status deployment/postgres --timeout=180s
kubectl -n telephony rollout status deployment/main-sip-gateway --timeout=240s
kubectl -n telephony wait --for=condition=Ready sipuser/alice sipuser/bob --timeout=180s
kubectl -n telephony get service main-sip-gateway main-rtpengine-0
```

Register two SIP clients against the external address assigned to the
`main-sip-gateway` Service.

| Phone | SIP username | Password | Extension |
| --- | --- | --- | --- |
| Alice | `alice` | `alice-demo-password` | `100` |
| Bob | `bob` | `bob-demo-password` | `101` |

Alice can call Bob at `101`; Bob can call Alice at `100`.

To change an extension later, use the CLI to merge the update into the live
resource:

```bash
uvx kubevoip --schema-source cluster -n telephony user update alice --extension 102 --caller-id "Alice <102>"
```

The quickstart uses a test PostgreSQL Deployment. Choose a production database
before running real users or trunks.
