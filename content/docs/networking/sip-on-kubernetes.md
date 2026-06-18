---
title: "SIP on Kubernetes"
weight: 50
description: "How KubeVoIP routes SIP signaling and RTP media through Kubernetes using Kamailio gateways and RTPengine media relay replicas."
url: "/networking/sip-on-kubernetes/"
---

SIP clients and trunks send requests to a `SIPGateway`. Kamailio authenticates
callers, selects routes from PostgreSQL runtime data, and asks RTPengine to
rewrite SDP for media relay. Calls can target registered SIP users, outbound
trunks, or Asterisk application pods.

For external traffic, use `NetworkProfile` and component-level external address
settings to control what SIP and RTP endpoints advertise.

For the default path, continue with
[LoadBalancer networking](/networking/loadbalancer/).
