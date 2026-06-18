---
title: "Secrets"
description: "How KubeVoIP uses Kubernetes Secrets for SIP users, SIP trunks, outbound caller ID, digest authentication, and database access."
url: "/operations/secrets/"
---

# Secrets

KubeVoIP reads Secrets only in the operator installation namespace. SIP user
passwords, trunk digest credentials, caller ID values, and database credentials
should stay out of Git and ConfigMaps.

Rendered ConfigMaps, statuses, Events, and logs must not expose raw passwords
or digest HA1 values.
