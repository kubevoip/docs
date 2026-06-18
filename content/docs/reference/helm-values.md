---
title: "Helm values"
description: "Common KubeVoIP Helm chart values for installing the namespace-scoped operator and configuring image repositories and tags."
---

# Helm values

The Helm chart exposes image repositories and tags for the operator and runtime
components, plus standard Kubernetes settings such as ServiceAccount, RBAC,
resources, node selectors, tolerations, and affinity.

The chart pins tested runtime images. Override image values only when testing a
specific component build.
