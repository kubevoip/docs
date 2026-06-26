---
title: "Kubernetes VoIP quickstart"
linkTitle: "Two-phone quickstart"
weight: 20
description: "Create a KubeVoIP SIP platform with two phones that can call each other through Kamailio and RTPengine on Kubernetes."
url: "/getting-started/quickstart/"
---

This quickstart creates a small SIP on Kubernetes deployment with two users:
`alice` on extension `100` and `bob` on extension `101`. Helm installs the
operator, and the `kubevoip` CLI creates a temporary database, base platform,
SIP users, dial policy, and routes.

## Prerequisites

- A Kubernetes cluster that can run LoadBalancer Services for UDP traffic.
- Helm 3.
- `uvx`, `pipx`, or another way to run the `kubevoip` CLI.
- Two SIP clients that can reach the gateway address.

Plain Kind clusters do not include a LoadBalancer controller. The quickstart
resources still reconcile, but `SIPGateway` and `MediaRelay` will wait for
external addresses until you add a controller such as cloud provider
LoadBalancer support or MetalLB.

## Install and initialize

```bash
helm repo add kubevoip https://charts.kubevoip.com
helm repo update
helm install kubevoip kubevoip/kubevoip \
  --version 0.6.7 \
  --namespace telephony --create-namespace

uvx kubevoip -n telephony init

kubectl -n telephony rollout status deployment/postgres --timeout=180s
kubectl -n telephony rollout status deployment/main-sip-gateway --timeout=240s
kubectl -n telephony wait --for=condition=Ready sipuser/alice sipuser/bob --timeout=180s
kubectl -n telephony get service main-sip-gateway main-rtpengine-0
```

The `init` command creates a test PostgreSQL Deployment, a `NetworkProfile`,
one `SIPGateway`, one `MediaRelay`, one internal `CallScope`, one
`DialPolicy`, two `SIPUser` resources, and two `CallRoute` resources.

## Register phones

Register two SIP clients against the external address assigned to the
`main-sip-gateway` Service.

| Phone | SIP username | Password | Extension |
| --- | --- | --- | --- |
| Alice | `alice` | `alice-demo-password` | `100` |
| Bob | `bob` | `bob-demo-password` | `101` |

Alice can call Bob at `101`; Bob can call Alice at `100`.

If registration fails, check:

```bash
kubectl -n telephony get sipgateway main
kubectl -n telephony get mediarelay main
kubectl -n telephony describe sipuser alice
```

For one-way audio, read [LoadBalancer networking](/networking/loadbalancer/)
and confirm UDP `5060` and the RTPengine media range are forwarded without port
translation.

## Change an extension

To change an extension later, use the CLI to merge the update into the live
resource:

```bash
uvx kubevoip -n telephony user update alice --extension 102 --caller-id "Alice <102>"
```

The operator reconciles the updated `SIPUser` into PostgreSQL. Kamailio reads
runtime user and route data from PostgreSQL, so normal user updates do not roll
the gateway pods.

## Use an existing PostgreSQL database

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

For a real deployment, continue with [Production setup](/operations/production/)
and [Secrets](/operations/secrets/) before adding phones or trunks you care
about.

## Clean up

When you are done, follow [Cleanup](/getting-started/cleanup/).
