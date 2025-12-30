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
Step 2 - Confiture IP addressing (Layer 3 only)

IP plan
R1 <---> R3: 10.0.13.0/30

R2 <---> R3: 10.0.23.0/30

Configure interfaces:
On R1-ISP-A:

```
interface FastEtherneet0/0
  ip address 10.0.13.1 255.255.255.252
  no shutdown
```
On R3-Customer:
```
interface FastEthernet0/0
  ip address 10.0.13.2 255.255.255.252
  no shutdown

interface FastEthernet0/1
  ip address 10.0.23.2 255.255.255.252
  no shutdown
```
On R2-ISP-B
```
interface FastEthernet0/0
ip address 10.0.23.1 255.255.255.252
no shutdown
```
Verify on all routers
```
show ip interface brief
ping 10.0.13.2
ping 10.0.23.2
```
















