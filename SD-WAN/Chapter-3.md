# Control Plane Operations

- vSmart is responsible for control plane functionality and facilitates Overlay Management Protocol (OMP) for building
the control plane.
- Control plane tunnels are encrypted and authenticated via Datagram Transport Layer Security (DTLS) or Transport
Layer Security (TLS).
- By default, DTLS is used over UDP port 12346.
  - vSmart and vManage use core-to-port mappings to distribute control connection load accross the CPU.

    | Core   	| UDP Port 	|
    |--------	|----------	|
    | Core 0 	| 12346    	|
    | Core 1 	| 12446    	|
    | Core 2 	| 12546    	|
    | Core 3 	| 12646    	|
    | Core 4 	| 12746    	|
    | Core 5 	| 12846    	|
    | Core 6 	| 12946    	|
    | Core 7 	| 13046    	|
  - Some common protocols can be used inside the tunnel are OMP, SNMP, and Netconf.

## Overlay Management Protocol
OMP is used to facilitates building the control plane and provides the following services:
- Facilitates network communication, including data plane connectivity among sites, service chaining, and multi-VPN
topology information.
- Advertises services available to the fabric.
- Distributes data plane security information: encryption keys.
- Best-path selection and routing policy advertisement.

OMP is enabled by default. OMP can interacts with all routing protocols including static routes, OSPF, BGP, EIGRP.
OMP operates very similarly to a BGP route reflector in an iBGP domain (Peering only between WAN Edges and vSmart).

OMP supports graceful restart. 
- When the connection to vSmart fails, WAN Edge routers continue to use the cached routing information with a default timer of 12 hours.
- This timer can be set between 1 second and 7 days.
- Recommend to set IPsec rekey timer to be twice the gracefull restart timer.

OMP advertises three types or routes: OMP routes, TLOC routes, service routes.

### OMP routes

OMP can advertise connected, static routes, and redistributed routes from OSPF, EIGRP, BGP to the vSmart controllers.
Along with reachability information, the following attributes are also adveritsed:
- TLOC: is the combination of
  - System IP address: used to identify the WAN edge that originally advertised the route.
  - Color: used to mark a WAN transport connection.
  - Encapsulation Type: GRE or IPsec.
- Origin: source of the route, with origin-proto and origin-metric, is used in best-path selection.
- Originator: identifies where the route was originally learned from, the system IP of the advertiser.
- Preference: OMP preference, can be modified to influence the best-path selection, a higher perference is preffered.
- Service: Cisco SD-WAN supports service insertion, firewall.
- Site ID: similar to BGP ASN
- Tag: optional, transitie attribute, redistributing to or from OMP does not carry the tag.
- VPN: indicates what VPN/VRF this route was advertised from

An example of OMP routes is as follow:
```bash
vEdge1# sh omp routes 10.12.0.0/24

---------------------------------------------------
omp route entries for vpn 1 route 10.12.0.0/24
---------------------------------------------------
            RECEIVED FROM:                   
peer            192.168.255.112
path-id         1
label           1003
status          C,I,R
loss-reason     not set
lost-to-peer    not set
lost-to-path-id not set
    Attributes:
     originator       192.168.255.2
     type             installed
     tloc             192.168.255.2, biz-internet, ipsec
     ultimate-tloc    not set
     domain-id        not set
     overlay-id        1
     site-id          2
     preference       not set
     tag              not set
     origin-proto     connected
     origin-metric    0
     as-path          not set
     community        not set
     unknown-attr-len not set
```

**Note that**: 
- `C, I, R` stands for "chosen, installed, resolved".
- OMP uses an administrative distance of 250 on Viptela OS routes, 251 on XE SD-WAN routes.

### TLOC routes

TLOC (Transport Location Identifier) routes identify the physical location of this device on that transport. TLOC is 
routable to the underlay and is the endpoint of the data plane tunnel.

TLOC = system IP + color + encapsulation type.

There are 22 predefined colors.

A TLOC route advertises the following information:
- TLOC private address: private IP address derived from the physical interface of the WAN Edge.
- TLOC public address: contains the publicly routable IP address assigned to the WAN Edge. If TLOC private and public
are identical, then the device is not be behind a NAT.
- Color: mark the WAN transport connection
- Encapsulation type: GRE or IPsec
- Preference: similar to OMP preference, this attribute allows to prefer one TLOC over another TLOC, a higher 
Preference value is preffered.
- Site ID: identifies the originator of this TLOC route.
- Tag: similar to route tags and OMP tags.
- Weight: used for path selection, similar to BGP Weight and is locally significant.


An example of TLOC route is as follow:

```bash
vEdge1# show omp tlocs detail 

---------------------------------------------------
tloc entries for 192.168.255.2
                 biz-internet
                 ipsec
---------------------------------------------------
            RECEIVED FROM:                   
peer            192.168.255.112
status          C,I,R
loss-reason     not set
lost-to-peer    not set
lost-to-path-id not set
    Attributes:
     attribute-type    installed
     encap-key         not set
     encap-proto       0
     encap-spi         256
     encap-auth        sha1-hmac,ah-sha1-hmac
     encap-encrypt     aes256
     public-ip         155.48.2.2
     public-port       12346
     private-ip        155.48.2.2
     private-port      12346
     public-ip         ::
     public-port       0
     private-ip        ::
     private-port      0
     bfd-status        up
     domain-id         not set
     site-id           2
     overlay-id        not set
     preference        0
     tag               not set
     stale             not set
     weight            1
     version           3
    gen-id             0x80000009
     carrier           default
     restrict          0
     on-demand          0
     groups            [ 0 ]
     bandwidth         0
     qos-group         default-group
     border             not set
     unknown-attr-len  not set
```

### Service Routes

Service Routes advertise a specific service to the rest of the overlay and can be used for Service Chaining. Note
that the Devices that provide services for the overlay must be Layer 2 adjacent.

To enable the Service Chaining, the workflow:
- Defines the service via feature template.
- WAN Edge routers advertise services to vSmart, also their OMP and TLOC routes.
- Applies a policy defining traffic must flow through these advertised services.

The service routes contain the following information:
- VPN ID: what VPN this service applies to.
- Service ID: defines service types: FW (svc-id 1), IDS (svc-id 2), IDP (svc-id 3), netsvc1, 2, 3, 4.
- Label:
- Orginator ID: system IP of the node advertising the service.
- TLOC: where the service is located.
- Path ID: identifies the OMP path.

### Path Selection

1. Valid OMP route
2. Locally sourced OMP route
3. Lower administrative distance
4. Higher OMP Preference
5. Higher TLOC Preference
6. Higher Origin: compare the origin type in the following order (first match wins): connected, static, eBGP,
EIGRP internal, OSPF intra-area, OSPF inter-area, OSPF external, EIGRP external, iBGP, Unknown.
7. Lowest Origin metric
8. Highest System IP
9. Highest TLOC private address

vSmart can advertise up to 16 equal-cost routes. By default, advertise 4 equal-cost routes.

### OMP Route Redistribution and Loop Prevention

By default, OMP will automatically redistribute Connected, Static, OSPF intra-area, OSPF inter-area. Redistribution
of BGP, EIGRP, and OSPF external must be explicitly configured.

Rooting Loop occurs when tow or more routers have mutual redistribution from the WAN and the LAN routing protocols.
- OSPF Loop prevention with Down Bit.
- BGP Loop Prevention with Site of Origin (SoO): When the other WAN Edge receives BGP update from core network, 
the BGP update with the SoO matches its own site ID will be dropped.