---
title: "Install KubeVoIP"
weight: 10
description: "Install the KubeVoIP Helm chart from charts.kubevoip.com into a namespace-scoped Kubernetes operator release for SIP platform resources."
url: "/getting-started/install/"
---

Install the operator with Helm:

```bash
helm repo add kubevoip https://charts.kubevoip.com
helm repo update
helm install kubevoip kubevoip/kubevoip \
  --version 0.6.7 \
  --namespace telephony --create-namespace
```

The same chart is also published to GHCR OCI for existing automation:

```bash
helm install kubevoip oci://ghcr.io/kubevoip/charts/kubevoip \
  --version 0.6.7 \
  --namespace telephony --create-namespace
```

Platform releases publish the operator image first. The matching Helm chart is
published after a reviewed chart release step, so the image tag may appear
briefly before the chart version.

Each Helm release watches only its installation namespace. Install a separate
release for each telephony namespace. CRDs remain cluster-scoped Kubernetes
resources shared by all releases.

After the operator is running, continue with the
[two-phone quickstart](/getting-started/quickstart/).
