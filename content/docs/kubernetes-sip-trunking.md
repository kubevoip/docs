---
title: "Kubernetes SIP trunking"
weight: 940
excludeFromPager: true
description: "Configure provider-neutral SIP trunking on Kubernetes with KubeVoIP, including inbound trusted CIDRs and outbound caller ID policy."
---

KubeVoIP models trunks with `SIPTrunk` resources. Inbound trunks can trust
configured source CIDRs and use an inbound `DialPolicy`. Outbound trunks can
present caller ID and answer digest challenges.
