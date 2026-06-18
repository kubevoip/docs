---
title: "Secrets"
weight: 90
description: "How KubeVoIP uses Kubernetes Secrets for SIP users, SIP trunks, outbound caller ID, digest authentication, and database access."
url: "/operations/secrets/"
---

KubeVoIP reads Secrets only in the operator installation namespace. SIP user
passwords, trunk digest credentials, caller ID values, and database credentials
should stay out of Git and ConfigMaps.

Rendered ConfigMaps, statuses, Events, and logs must not expose raw passwords
or digest HA1 values.

## SIP device passwords

A `SIPUser` references a Secret instead of storing a password in the resource:

```bash
printf '%s' "$SIP_PASSWORD" | uvx kubevoip -n telephony secret sip-user alice-sip \
  --from-stdin
```

Then create or update the user with that Secret:

```bash
uvx kubevoip -n telephony user create alice \
  --extension 100 \
  --gateway main \
  --dial-policy internal-only \
  --auth-username alice \
  --caller-id "Alice <100>" \
  --password-secret alice-sip
```

The Secret defaults to key `password`. Use `--key` on `secret sip-user` and
`--password-key` on `user create` if your Secret uses a different key.

## Operational guidance

- Prefer random per-device SIP passwords.
- Store generated manifests in Git only when they do not contain Secret values.
- Use External Secrets, Sealed Secrets, SOPS, or your existing secret-management
  workflow for production GitOps.
- Rotate a SIP password by updating the Secret; the operator reconciles the new
  password-derived HA1 values into PostgreSQL.
