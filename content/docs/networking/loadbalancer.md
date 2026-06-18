---
title: "LoadBalancer networking"
description: "Use Kubernetes LoadBalancer Services for KubeVoIP SIP signaling and RTP media traffic while preserving public and private UDP ports."
url: "/networking/loadbalancer/"
---

# LoadBalancer networking

`LoadBalancer` mode lets Kubernetes or your infrastructure allocate addresses
for SIP and RTP Services. If KubeVoIP cannot resolve an address yet, it reports
a waiting condition until the Service ingress is available.

Do not translate SIP or RTP ports. Forward UDP `5060` to the gateway and each
RTPengine media range to its corresponding relay address.
