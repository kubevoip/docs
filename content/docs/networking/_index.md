---
title: "Networking"
description: "How KubeVoIP handles external SIP and RTP networking on Kubernetes with LoadBalancer Services, HostNetwork mode, and public addresses."
url: "/networking/"
---

# Networking

KubeVoIP rewrites SIP advertised addresses and RTPengine SDP addresses. Public
forwarding needs identical public and private UDP ports: SIP `5060` and each
assigned RTPengine range.
