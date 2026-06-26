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

For SIP routing issues, inspect Kamailio summary logs:

```bash
kubectl -n telephony logs deploy/main-sip-gateway -c kamailio \
  | grep kubevoip_sip_event
```

For one-way audio, check RTPengine Service addresses, firewall rules, and RTP
port forwarding. SIP and RTP public ports must match the private ports.
RTPengine startup and runtime logs are available from the relay pods:

```bash
kubectl -n telephony logs deploy/main-rtpengine-0 -c rtpengine
```

For deeper call-flow analysis, enable [HOMER HEP capture](/operations/observability/)
on the affected `SIPGateway` and search HOMER by SIP Call-ID.
