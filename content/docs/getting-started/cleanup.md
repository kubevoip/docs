---
title: "Cleanup"
description: "Remove a KubeVoIP quickstart installation from Kubernetes, including demo SIP users, routing resources, Services, and test database objects."
url: "/getting-started/cleanup/"
---

# Cleanup

Delete the quickstart resources and uninstall the Helm release:

```bash
kubectl delete -f https://raw.githubusercontent.com/kubevoip/kubevoip/main/examples/quickstart-two-phones.yaml
helm uninstall kubevoip --namespace telephony
```

Helm leaves CRDs installed. Remove CRDs only when no KubeVoIP resources remain
in the cluster.
