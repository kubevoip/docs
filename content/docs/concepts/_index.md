---
title: "Concepts"
description: "Understand the KubeVoIP resource model for SIP on Kubernetes, including gateways, media relays, users, trunks, scopes, and routes."
---

# Concepts

KubeVoIP models a SIP platform as Kubernetes resources. The core resources are
`SIPGateway`, `MediaRelay`, `SIPUser`, `SIPTrunk`, `CallScope`, `DialPolicy`,
`CallRoute`, and `AsteriskPool`.
