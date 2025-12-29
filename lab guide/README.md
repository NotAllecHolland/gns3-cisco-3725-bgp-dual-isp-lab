Step 1 - Prepare environment and topology
GNS3 version: 2.2.55
Routers: 3x Cisco 3725
Names:
  -R1-ISP-A    (AS 65001)
  -R2-ISP-B    (AS 65002)
  -R3 Customer (AS 65010

Used routed FastEthernet interfaces only (Fa0/x)
DOT NOT install NM-16ESW OR configure VLANs

Verify routers are up
```
show version

```

Step- Confiture IP addressing (Layer 3 only)
R1 ↔ R3: 10.0.13.0/30
