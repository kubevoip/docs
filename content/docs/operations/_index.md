---
title: "Operations"
weight: 40
description: "Operate KubeVoIP with PostgreSQL, Kubernetes Secrets, namespace-scoped installs, upgrades, troubleshooting, and runtime data safety."
url: "/operations/"
---

KubeVoIP stores routing data in PostgreSQL so Kamailio pods can be replaced
without losing the dial plan. Treat PostgreSQL and Secret-backed credentials as
part of the SIP platform security boundary.

Before adding real users or trunks, read [Production setup](/operations/production/)
so the database, Secrets, and dial policies are in the right shape.
