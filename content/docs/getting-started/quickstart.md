---
title: "Kubernetes VoIP quickstart"
description: "Create a KubeVoIP SIP platform with two phones that can call each other through Kamailio and RTPengine on Kubernetes."
---

# Kubernetes VoIP quickstart

This quickstart creates a small SIP on Kubernetes deployment with two users:
`alice` on extension `100` and `bob` on extension `101`.

```bash
helm install kubevoip oci://ghcr.io/kubevoip/charts/kubevoip \
  --version 0.5.0 \
  --namespace telephony --create-namespace

kubectl apply -f https://raw.githubusercontent.com/kubevoip/kubevoip/main/examples/quickstart-two-phones.yaml
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

The quickstart uses a test PostgreSQL Deployment. Choose a production database
before running real users or trunks.
