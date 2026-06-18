---
title: "Cleanup"
weight: 30
description: "Remove a KubeVoIP quickstart installation from Kubernetes, including demo SIP users, routing resources, Services, and test database objects."
url: "/getting-started/cleanup/"
---

If you used a dedicated `telephony` namespace for the quickstart, uninstall the
operator and delete the namespace:

```bash
helm uninstall kubevoip --namespace telephony
kubectl delete namespace telephony
```

If you used an existing namespace, delete only the quickstart objects instead:

```bash
kubectl -n telephony delete callroute user-100 user-101 --ignore-not-found
kubectl -n telephony delete sipuser alice bob --ignore-not-found
kubectl -n telephony delete dialpolicy internal-only --ignore-not-found
kubectl -n telephony delete callscope internal --ignore-not-found
kubectl -n telephony delete sipgateway main --ignore-not-found
kubectl -n telephony delete mediarelay main --ignore-not-found
kubectl -n telephony delete networkprofile public --ignore-not-found
kubectl -n telephony delete deployment postgres --ignore-not-found
kubectl -n telephony delete service postgres --ignore-not-found
kubectl -n telephony delete secret postgres-app alice-sip bob-sip --ignore-not-found
helm uninstall kubevoip --namespace telephony
```

Helm leaves CRDs installed. Remove CRDs only when no KubeVoIP resources remain
in the cluster.
