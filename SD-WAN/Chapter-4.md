# Onboarding and Provisioning
## Configuration Templates

When configure the WAN Edge devices or controllers using vManage GUI, we will attach a device template to that
devices. The device templates can be CLI-based or feature template-based.

- CLI-based: the whole configuration must be in the template.
- Feature template-based: contains multiple feature templates, each feature template is as a building block for
a specific technology feature. Feature template is device type agnostic.

Device templates have four main parts:
- Basic Information
- Transport and Management VPN
- Service VPN
- Additional Templates

Feature templates:
- make configuration options flexible
- provide option to define variables for configuration parameters, that allows to reduce the number of templates
required.

Three types of values:
- Default
- Global:
- Device specific:

Some common feature templates: System, BFD, OMP, Security, VPN, BGP, OSPF, VPN Interface.

Developing and Deploying Templates

## Onboarding Devices

To join SD-WAN fabric, the WAN Edge first needs to connect to vBond, then vBond will tell WAN Edge about vSmart and
vManage.

There are two methods to bootstrap a device:
- Manual Boostrapting:
- Automatic Provisioning with PNP or ZTP:

Automatic Provisioning with PNP and ZTP:
- One powered on, the devices tries to receive an IP address via DHCP.
- It will reach out to the automatic provisioning server and learn about vBond
- It will connect and authenticate with vBond, learn of vManage and vSmart, and then receives its configuration

