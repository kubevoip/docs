---
title: "Operations"
description: "Operate KubeVoIP with PostgreSQL, Kubernetes Secrets, namespace-scoped installs, upgrades, troubleshooting, and runtime data safety."
---

# Operations

KubeVoIP keeps normal routing data in PostgreSQL so Kamailio pods can stay
mostly disposable. Protect PostgreSQL and any Secret-backed credentials as part
of the SIP platform security boundary.
