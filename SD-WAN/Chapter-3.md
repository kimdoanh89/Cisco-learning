## Control Plane Operations

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

### Overlay Management Protocol
OMP is used to facilitates building the control plane and provides the following services:
- Facilitates network communication, including data plane connectivity among sites, service chaining, and multi-VPN
topology information.
- Advertises services available to the fabric.
- Distributes data plane security information: encryption keys.
- Best-path selection and routing policy advertisement.

OMP is enabled by default. OMP can interacts with all routing protocols including static routes, OSPF, BGP, EIGRP.
OMP operates very similarly to a BGP route reflector in an iBGP domain (Peering only between WAN Edges and vSmart).

OMP supports graceful restart. When the connection to vSmart fails, WAN Edge routers continue to use the cached routing
information 
