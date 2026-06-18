---
title: "Docs"
weight: 1
description: "KubeVoIP documentation for installation, SIP routing concepts, networking, operations, generated API reference, and contributor workflows."
---

Start here if you are installing KubeVoIP or trying to understand how the pieces
fit together. For a new setup, install the chart, run the two-phone quickstart,
then read the networking and PostgreSQL pages before adding real users or
trunks.

## Getting started

- [Install KubeVoIP](/getting-started/install/) with Helm OCI into a
  namespace-scoped operator release.
- [Run the two-phone quickstart](/getting-started/quickstart/) with the
  `kubevoip` CLI and place your first SIP call.
- [Clean up the quickstart](/getting-started/cleanup/) when you are done.

## Concepts

- [Platform resources](/concepts/platform-resources/) explains how
  `SIPGateway`, `MediaRelay`, `SIPUser`, `SIPTrunk`, `CallScope`,
  `DialPolicy`, `CallRoute`, and `AsteriskPool` fit together.

## Networking

- [SIP on Kubernetes](/networking/sip-on-kubernetes/) explains the signaling
  and media flow.
- [LoadBalancer networking](/networking/loadbalancer/) covers Services,
  external addresses, and UDP port preservation.
- [HostNetwork media mode](/networking/hostnetwork/) covers the node-level
  media path for clusters where Service NAT is not a fit.

## Operations

- [PostgreSQL](/operations/postgresql/) covers runtime data, migrations, and
  HA1 storage.
- [Secrets](/operations/secrets/) explains credential handling.
- [Production setup](/operations/production/) explains real SIP device
  credentials, production PostgreSQL, and dial policy.
- [Troubleshooting](/operations/troubleshooting/) gives the first commands to
  run when registration, routing, or media is not working.

## Reference

- [API reference](/reference/api/) is generated from the current CRDs.
- [Helm values](/reference/helm-values/) and
  [component images](/reference/components/) document release inputs.

## Contributing

- [Local development](/contributing/local-development/) and
  [release process](/contributing/release-process/) describe project workflow.
