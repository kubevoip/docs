---
title: "Troubleshooting"
weight: 105
description: "Troubleshoot KubeVoIP SIP registration, call routing, RTP media, database readiness, LoadBalancer addresses, and Kubernetes resources."
url: "/operations/troubleshooting/"
---

Start with resource status:

```bash
kubectl -n telephony get networkprofile,sipgateway,mediarelay,asteriskpool,sipuser,siptrunk,callscope,dialpolicy,callroute
kubectl -n telephony describe sipgateway main
kubectl -n telephony logs deployment/kubevoip-kubevoip
```

For one-way audio, check RTPengine Service addresses, firewall rules, and RTP
port forwarding. SIP and RTP public ports must match the private ports.
