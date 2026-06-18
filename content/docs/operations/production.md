---
title: "Production setup"
weight: 100
description: "Move KubeVoIP beyond the quickstart with production PostgreSQL, SIP device Secrets, route policy, networking, and operational checks."
url: "/operations/production/"
---

The quickstart is small on purpose: it creates a test PostgreSQL Deployment, two
demo users, one internal dial policy, and two routes. In production, keep the
same resource model, but replace the demo pieces with durable infrastructure and
real credentials.

## Use production PostgreSQL

KubeVoIP expects a PostgreSQL connection Secret with `host`, `port`, `dbname`,
`user`, and `password` keys. The database can come from a cloud provider,
CNPG, another PostgreSQL operator, or your own managed service.

Create the Secret without putting the password in shell history:

```bash
printf '%s' "$POSTGRES_PASSWORD" | uvx kubevoip -n telephony secret database postgres-app \
  --host "$POSTGRES_HOST" \
  --port 5432 \
  --db kubevoip \
  --user kubevoip \
  --password-stdin
```

Then create the gateway against that Secret, or initialize the platform with
that database:

```bash
printf '%s' "$POSTGRES_PASSWORD" | uvx kubevoip -n telephony init \
  --database existing \
  --postgres-host "$POSTGRES_HOST" \
  --postgres-db kubevoip \
  --postgres-user kubevoip \
  --postgres-password-stdin
```

PostgreSQL stores runtime routing data and HA1 password hashes. Treat HA1 as
credential-equivalent material and protect database backups accordingly.

## Create SIP device credentials

Each phone should have its own `SIPUser` and password Secret:

```bash
export SIP_PASSWORD="$(openssl rand -base64 32)"

printf '%s' "$SIP_PASSWORD" | uvx kubevoip -n telephony secret sip-user alice-sip \
  --from-stdin

uvx kubevoip -n telephony user create alice \
  --extension 100 \
  --gateway main \
  --dial-policy internal-only \
  --auth-username alice \
  --caller-id "Alice <100>" \
  --password-secret alice-sip
```

Configure the SIP client with:

| Field | Value |
| --- | --- |
| Server / registrar | The `main-sip-gateway` external address |
| Username | `alice` |
| Authentication username | `alice` |
| Password | The generated Secret value |
| Extension | `100` |

Do not commit raw SIP passwords to Git. If you use GitOps, store credentials
through your normal secret-management system and create Kubernetes Secrets from
there.

## Model who can call what

Use `CallScope`, `DialPolicy`, and `CallRoute` to express the dial plan:

```bash
uvx kubevoip -n telephony scope create internal --gateway main

uvx kubevoip -n telephony policy create internal-only \
  --gateway main \
  --scope internal

uvx kubevoip -n telephony route create bob-extension \
  --gateway main \
  --scope internal \
  --priority 10 \
  --match 101 \
  --target-user bob
```

A caller can only search scopes listed in its `DialPolicy`. Put outbound trunks
or application routes in separate scopes when only some callers should use
them.

## Expose SIP and RTP deliberately

Before accepting real traffic, read [SIP on Kubernetes](/networking/sip-on-kubernetes/)
and [LoadBalancer networking](/networking/loadbalancer/). Public SIP and RTP
forwarding must preserve ports. KubeVoIP does not configure provider portals,
DNS, routers, cloud firewall rules, or perimeter firewalls.

## Production checklist

- Use a durable PostgreSQL database and backups.
- Protect PostgreSQL because it stores credential-equivalent HA1 values.
- Use unique SIP credentials per device.
- Keep Secrets out of ConfigMaps and Git.
- Confirm `SIPGateway`, `MediaRelay`, `SIPUser`, `CallScope`, `DialPolicy`, and
  `CallRoute` resources report Ready.
- Confirm LoadBalancer addresses and firewall rules before testing real calls.
- Document which dial policies are allowed to reach trunks.
