---
title: "Platform resources"
description: "A practical overview of KubeVoIP Kubernetes resources and how SIPGateway, MediaRelay, SIPUser, SIPTrunk, and CallRoute fit together."
---

# Platform resources

- `NetworkProfile` defines shared external addressing and local networks.
- `SIPGateway` manages Kamailio registration, routing, and SIP edge behavior.
- `MediaRelay` manages RTPengine replicas and media port ranges.
- `AsteriskPool` manages private Asterisk application pods.
- `SIPUser` represents a phone identity registered through Kamailio.
- `SIPTrunk` represents provider-neutral inbound and outbound trunk policy.
- `CallScope` groups searchable routes.
- `DialPolicy` defines which scopes a caller can search.
- `CallRoute` routes calls to users, trunks, or Asterisk applications.

See the [API reference](/reference/api/) for generated field details.
