---
title: "HostNetwork media mode"
weight: 70
description: "Use HostNetwork mode for RTPengine when Kubernetes Service NAT is not suitable and node-level UDP media forwarding is preferred."
url: "/networking/hostnetwork/"
---

`HostNetwork` avoids Kubernetes Service NAT for media but binds RTPengine ports
directly on selected nodes. The cluster operator must handle node selection,
public addresses, firewall rules, and port conflicts.

Use this mode when media packets need to land directly on selected Kubernetes
nodes instead of passing through a Service. It can be useful in environments
where UDP Service behavior is hard to control, but it moves more responsibility
to node placement and firewall configuration.

Start with [LoadBalancer networking](/networking/loadbalancer/) unless you
already know Service NAT is the wrong fit for your cluster.
