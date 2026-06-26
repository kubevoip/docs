---
title: "Helm values"
weight: 130
description: "Common KubeVoIP Helm chart values for installing the namespace-scoped operator and configuring image repositories and tags."
url: "/reference/helm-values/"
---

The Helm chart exposes image repositories and tags for the operator and runtime
components, plus standard Kubernetes settings such as ServiceAccount, RBAC,
resources, node selectors, tolerations, and affinity.

The chart pins tested runtime images. Override image values only when testing a
specific component build.

Operator failover HA is opt-in:

```yaml
operator:
  replicas: 3
  highAvailability:
    enabled: true
    peeringName: ""
```

When HA is enabled and `peeringName` is empty, the chart uses the release
fullname. Kopf peering keeps the operator active/passive: one pod reconciles
while standby pods wait for failover, provided generated priorities are unique.
The chart derives priorities from pod IPs, so they are probabilistically unique
and intended for small replica counts, normally 2 or 3.

This is controller failover HA only. It does not make Kamailio, RTPengine,
Asterisk, PostgreSQL, or SIP/RTP traffic handling highly available. The Kopf
peering CRDs are cluster-scoped resources shared by all KubeVoIP releases, even
though each release creates a namespaced `KopfPeering`.
