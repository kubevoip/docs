---
title: "Platform resources"
weight: 40
description: "A practical overview of KubeVoIP Kubernetes resources and how SIPGateway, MediaRelay, SIPUser, SIPTrunk, and CallRoute fit together."
url: "/concepts/platform-resources/"
---

- `NetworkProfile` defines shared external addressing and local networks.
- `SIPGateway` manages Kamailio registration, routing, and SIP edge behavior.
- `MediaRelay` manages RTPengine replicas and media port ranges.
- `AsteriskPool` manages private Asterisk application pods.
- `SIPUser` represents a phone identity registered through Kamailio.
- `SIPTrunk` represents provider-neutral inbound and outbound trunk policy.
- `CallScope` groups searchable routes.
- `DialPolicy` defines which scopes a caller can search.
- `CallRoute` routes calls to users, trunks, or Asterisk applications.

## Routing model

KubeVoIP separates caller permission from route matching:

- `CallScope` is a bucket of routes, such as `internal` or `external`.
- `DialPolicy` is the ordered list of scopes a caller may search.
- `CallRoute` is one callable pattern inside a scope.

When a SIP user places a call, Kamailio finds that user's `DialPolicy`, searches
the referenced scopes in order, and then chooses the best `CallRoute` inside
the first matching scope. Lower route priority wins, and ties sort by resource
name.

That means two users can see different dial plans without duplicating the whole
platform. For example, extension `100` can use a policy with `internal` and
`external` scopes, while extension `101` can use a policy with only the
`internal` scope.

## Minimal internal calling example

An internal-only setup needs:

- one `CallScope`, for example `internal`;
- one `DialPolicy` that references `internal`;
- one `SIPUser` per phone, each pointing at the policy;
- one `CallRoute` per reachable extension.

For Alice on `100` and Bob on `101`, the route for Bob would match `101` and
target `sipUserRef: bob`. The route for Alice would match `100` and target
`sipUserRef: alice`.

## External and application routes

`CallRoute` targets can also point at:

- `trunkRef` for provider-neutral outbound SIP trunking;
- `asteriskPoolRef` plus an extension for application calls such as Echo.

Put those routes in a separate scope, such as `external` or `applications`, when
not every caller should be allowed to use them.

See the [API reference](/reference/api/) for generated field details.
