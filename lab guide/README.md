Step 1
Prepare environment and topology

GNS3 version: 2.2.55

Routers:
3x Cisco 3725

Names:

  R1-ISP-A    (AS 65001)
  
  R2-ISP-B    (AS 65002)
  
  R3-Customer (AS 65010)

Used routed FastEthernet interfaces only (Fa0/x)
DO NOT install NM-16ESW OR configure VLANs

Verify routers are up
```
show version
```
Step 2 
Configure IP addressing (Layer 3 only)

IP plan

R1 <---> R3: 10.0.13.0/30

R2 <---> R3: 10.0.23.0/30

Configure interfaces:

On R1-ISP-A:

```
interface FastEthernet0/0
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
Verify
```
show ip interface brief
ping 10.0.13.2
ping 10.0.23.2
```
Step 3 
Configure eBGP neighbors

R1-ISP-A

```
router bgp 65001
 neighbor 10.0.13.2 remote-as 65010
```
R2-ISP-B
```
router bgp 65002
neighbor 10.0.23.2 remote-as 65010
```
R3-Customer
```
router bgp 65010
  neighbor 10.0.13.1 remote-as 65001
  neighbor 10.0.23.1 remote-as 65002
```
Verify
```
show ip bgp summary
```
Expected result: All neighbors Established.

Step 4
Advertise in BGP
R3-Customer advertises connected networks 

```
router bgp 65010
network 10.0.13.0 mask 255.255.255.252
network 10.0.23.0 mask 255.255.255.252
```
ISPs advertise a default route 

```
ip route 0.0.0.0 0.0.0.0 Null0

router bgp 65001
 network 0.0.0.0
```
```
ip route 0.0.0.0 0.0.0.0 Null0

router bgp 65002
network 0.0.0.0
```
Verify
```
show ip bgp
show ip bgp 0.0.0.0
```

Step 5
Influence path selection with Local Preference

Prefer ISP-A on R3-Customer
```
route-map PREF-ISP-A permit 10
 set local-preference 200

router bgp 65010
 neighbor 10.0.13.1 route-map PREF-ISP-A in
```
Apply immediately
```
clear ip bgp *
```
Verify
```
show ip bgp 0.0.0.0
show ip route 0.0.0.0
```
Route 10.0.13.1 preferred 

Step 6
Test failover and convergence
Simulate ISP-A failure
```
interface FastEthernet0/0
 shutdown
```
Verify failover on R3
```
show ip bgp summary
show ip route 0.0.0.0
```
Default route now switched to 10.0.23.1

```
interface FastEthernet0/0
 no shutdown
```
Notes/Lessons Learned:

NM-16ESW is Layer 2 only which causes VLAN/VTP/flash errors in Dynamips

VLANs and SVIs are not required for BGP

Only Fa0/x routed interfaces were used. 

This created a stable, repeatable lab

