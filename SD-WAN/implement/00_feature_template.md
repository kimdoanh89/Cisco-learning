## 1. Create Feature Template
Before starting create template, it is very important to plan the template name properly. We need to come up with
 a meaningful way to name the template that is suitable for the intent use of the template.
- Naming convention: 
  - System template for vEdge Cloud at a Branch: BR-VE-SYSTEM
  - VPN template for vEdge Cloud at a Branch: BR-VE-VPN-VPN0

When we create a template, for each parameter we have three types of setting as follows:
- Global: Apply to all devices using this template.
- Device specific: when apply template, we will need to specify this parameter.
- Default:
### Create System Template
We create the first `BR-VE-SYSTEM` template as in the following figure.
![text](images/BR-VE-SYSTEM.PNG)
### Create VPN0 Template
Basic configuration:

![text](images/BR-VE-VPN-VPN0-basic.PNG)

IPv4 Route configuration:

![text](images/BR-VE-VPN-VPN0-IPv4.PNG)
### Create VPN Interface Ethernet BR-VE-VPNINT-VPN0-G0
Basic configuration:

![text](images/BR-VE-VPNINT-VPN0-G0-basic.PNG)

Tunnel configuration:
- Turn on tunnel interface.
- Select color as `mpls`.

![text](images/BR-VE-VPNINT-VPN0-G0-tunnel1.PNG)

![text](images/BR-VE-VPNINT-VPN0-G0-tunnel-allow-service.PNG)
