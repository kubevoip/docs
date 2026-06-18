---
title: "PostgreSQL"
weight: 80
description: "PostgreSQL stores KubeVoIP runtime routing, SIP registration, dial policy, trunk, caller ID, and digest HA1 data for Kamailio."
url: "/operations/postgresql/"
---

KubeVoIP requires a PostgreSQL database and a standard connection Secret with
`host`, `port`, `dbname`, `user`, and `password`.

The operator runs forward database migrations automatically during
reconciliation. PostgreSQL stores route data, dial policies, subscriber hashes,
registrations, trunk metadata, and digest HA1 values. HA1 is
credential-equivalent for its SIP realm.

Use a durable PostgreSQL service for production instead of the quickstart
Deployment. Point `SIPGateway.spec.databaseSecretRef.name` at the Secret for
that database.
