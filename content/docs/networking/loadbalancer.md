---
title: "LoadBalancer networking"
weight: 60
description: "Use Kubernetes LoadBalancer Services for KubeVoIP SIP signaling and RTP media traffic while preserving public and private UDP ports."
url: "/networking/loadbalancer/"
---

`LoadBalancer` mode lets Kubernetes or your infrastructure allocate addresses
for SIP and RTP Services. If KubeVoIP cannot resolve an address yet, it reports
a waiting condition until the Service ingress is available.

Use this mode when your cluster or network can provide stable UDP
LoadBalancer addresses. Cloud load balancers, MetalLB, and similar controllers
can all fit this model as long as they preserve the configured UDP ports.

## What gets exposed

KubeVoIP exposes signaling and media separately:

- `SIPGateway` owns the Kamailio Service for SIP signaling, normally UDP
  `5060`.
- `MediaRelay` owns one Service per RTPengine replica. Each replica receives a
  stable slice of the configured RTP port range.
- `NetworkProfile` tells KubeVoIP which external address to advertise, or lets
  KubeVoIP discover addresses from LoadBalancer Service status.

SIP clients and trunks send SIP to the gateway address. Kamailio asks
RTPengine to rewrite SDP so media flows through the selected RTPengine replica.

## Port preservation

Do not translate SIP or RTP ports. Public and private ports need to match:

- UDP `5060` to the SIP gateway Service.
- Each RTPengine media range to its matching RTPengine Service.

Port translation can leave SIP clients, providers, or RTPengine advertising one
address and port while packets arrive somewhere else.

## Waiting for addresses

If a Service has no `status.loadBalancer.ingress` address yet, KubeVoIP reports
a waiting condition. This is normal on clusters without LoadBalancer support.
For example, a plain Kind cluster will usually leave `SIPGateway` and
`MediaRelay` waiting until you install a LoadBalancer controller or switch to a
different networking mode.

Check the current addresses with:

```bash
kubectl -n telephony get service main-sip-gateway main-rtpengine-0
kubectl -n telephony get sipgateway main
kubectl -n telephony get mediarelay main
```

## Infrastructure responsibilities

KubeVoIP renders Kubernetes resources and advertised SIP/RTP addresses. Your
infrastructure still owns public DNS, firewall rules, router forwarding, cloud
load balancer annotations, and provider-side trunk settings.

For a working first call, run the
[two-phone quickstart](/getting-started/quickstart/) and then come back here
before exposing SIP or RTP outside the cluster.
