---
title: "Networking"
weight: 30
description: "How KubeVoIP handles external SIP and RTP networking on Kubernetes with LoadBalancer Services, HostNetwork mode, and public addresses."
url: "/networking/"
---

KubeVoIP rewrites SIP advertised addresses and RTPengine SDP addresses. Public
forwarding needs identical public and private UDP ports: SIP `5060` and each
assigned RTPengine range.

Start with [SIP on Kubernetes](/networking/sip-on-kubernetes/) for the packet
flow, then choose either [LoadBalancer networking](/networking/loadbalancer/)
or [HostNetwork media mode](/networking/hostnetwork/) for media exposure.
