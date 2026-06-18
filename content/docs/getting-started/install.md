---
title: "Install KubeVoIP"
weight: 10
description: "Install the KubeVoIP Helm chart from GHCR OCI into a namespace-scoped Kubernetes operator release for SIP platform resources."
url: "/getting-started/install/"
---

Install the operator with Helm:

```bash
helm install kubevoip oci://ghcr.io/kubevoip/charts/kubevoip \
  --version 0.5.0 \
  --namespace telephony --create-namespace
```

Each Helm release watches only its installation namespace. Install a separate
release for each telephony namespace. CRDs remain cluster-scoped Kubernetes
resources shared by all releases.

After the operator is running, continue with the
[two-phone quickstart](/getting-started/quickstart/).
