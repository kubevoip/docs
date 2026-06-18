---
title: "HostNetwork media mode"
description: "Use HostNetwork mode for RTPengine when Kubernetes Service NAT is not suitable and node-level UDP media forwarding is preferred."
---

# HostNetwork media mode

`HostNetwork` avoids Kubernetes Service NAT for media but binds RTPengine ports
directly on selected nodes. The cluster operator must handle node selection,
public addresses, firewall rules, and port conflicts.
