##This lab demonstrates a realistic dual-ISP eBGP setup using Cisco 3725 routers in GNS3, focusing on how the Internet traffic actually routes traffic. Due to issues with NM-16ESW interfaces being only Layer 2 which causes VLAN/VTP/flash errors in dynamips, I designed the lab to explicitly use routed FastEthernet interfaces (Fa0/x) only to avoid switch modules, VLANs, and SVIs to keep the lab stable and accurate. The goal is to model a customer network connected to two different ISPs. Each one having it's own Autonomous System, just like real world internet edge designs. 

##The customer establishes separate eGBP sessions. Each IPS uses explicit AS number and neighbor definitions. The customer can learn the same destination from two different paths due to both ISPs advertising a default route. This allows me to see how BGP exchanges routes, tracks AS paths and how it installs new routes into the routing table. 

##To control outbound traffic, Local Preference is used which forces the customer to prefer one ISP over the other. To prove that BGP automatically fails over when a preferred path disappears, I simulated a link failure by disabling a port in the CLI. 

Key lessons this lab highlights:
-BGP is policy-driven

-Routed interfaces matter

-Clean design choices lead to predictable, repeatable behaviors. 
