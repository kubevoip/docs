---
title: "Kubernetes VoIP quickstart"
description: "Create a KubeVoIP SIP platform with two phones that can call each other through Kamailio and RTPengine on Kubernetes."
url: "/getting-started/quickstart/"
---

# Kubernetes VoIP quickstart

This quickstart creates a small SIP on Kubernetes deployment with two users:
`alice` on extension `100` and `bob` on extension `101`. Helm installs the
operator, and the `kubevoip` CLI creates a temporary database, base platform,
SIP users, dial policy, and routes.

```bash
helm install kubevoip oci://ghcr.io/kubevoip/charts/kubevoip \
  --version 0.5.0 \
  --namespace telephony --create-namespace

uvx kubevoip -n telephony init

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
uvx kubevoip -n telephony user update alice --extension 102 --caller-id "Alice <102>"
```

The quickstart uses a test PostgreSQL Deployment. Choose a production database
before running real users or trunks.

If you already have PostgreSQL, use `init --database existing` instead:

```bash
printf '%s' "$POSTGRES_PASSWORD" | uvx kubevoip -n telephony init \
  --database existing \
  --postgres-host "$POSTGRES_HOST" \
  --postgres-db kubevoip \
  --postgres-user kubevoip \
  --postgres-password-stdin
```
