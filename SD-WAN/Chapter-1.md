========================================================

Networks of today

- New technologies: AI, ML, Cloud Services, Virtualization, IoT are putting strain on the IT operation staff.
- These technologies offer:
  - Automation
  - Improving overal user experience within environment
  - Simplify operations of the network
- Risks associated with manually configuring Networks:
  - not being able to move fast enough when deploying new applications or Services
  - Misconfigurations
  - not being able to keep up with the scalability demand
- Analogy: Automobile as a System
- Over 30 years, thinking of a network as a collection of devices: routers, SWs, wireless components.
- Shift: Network as a holistic system
  - SDN and controllers offer the ability to manage network as a system
  - policy management can be automated and abstracted

========================================================

Common Business and IT trend

- Enterprises have different requirements: high bandwidth, real-time and big data applications
- A surge in bring-your-own-device (BYOD), cloud and dynamic business-to-business (B2B) ecosystems
- Huge increase in the use of Software as a Service (SaaS) and Infrastructure as a Service (IaaS)
  - Microsoft Office 365, Google Apps, etc.
- Common trends being seen in the industry:
  - Applications are moving to the cloud (private and public)
  - Internet edge is moving to the remote branch sites
  - Mobile devices (BYOD and guest access)
  - High-bandwidth applications
  - IoT devices
- Solutions: Offloading certain types of traffic and active/active WAN deployment models

========================================================

Common Desired Benefits

- Benefits:
  - Prioritize and secure traffic with granular control
  - Reduce costs and lower operational complexity
  - Augment or replace premium WAN bandwidth
  - Provide a consistent, high-quality user experience
  - Offload guest and public cloud traffic
  - Ensure remote site uptime
- Providing the following business outcomes and use cases:
  - Faster branch deployment with no operational interaction
  - Complete end-to-end network segmentation for enhanced security and privacy
  - Increased WAN performance
  - Topology independence
  - Better user experience

========================================================

High-Level Designed Considerations

- Various topologies and redundancy types

![text](images/figure-1.3.png)
- Some other aspects that cause complexity within network: securing the network, leveraging network segmentation (to keep traffic types separate), implementing QoS.
- Intent-based Networking (IBN): signifying the intent of the business and automatically translating that intent into the appropriate corresponding networking tasks.
- Digital Transformation Transition

![text](images/figure-1.4.png)

========================================================

Introduction to Cisco SD-WAN

- Hybrid WAN:
  - Additional non-MPLS commodity links (Internet, L2VPN, wireless, 4G/LTE) are added to the WAN to provide alternative paths.
  - provides transport independence.
  - some applications can be sent over these commodity links versus the service provider-controlled L3VPN MPLS links.
  - WAN connectivity is based on the network topology and managed using a peer-to-peer model. Routing (OSPF, BGP) and security (IPsec) control planes
  run independently of each other. When a configuration change is required, it has to be provisioned and propagated across all the control plane peers.

- Transport independence:
  - Cisco SD-WAN leverages a transport-independent fabric topology that is used to connect remote locations together.
    - 

========================================================

Use cases demanding Changes in the WAN

========================================================

Building an ROI to Identify Cost Savings

========================================================

Introduction to Multidomain
