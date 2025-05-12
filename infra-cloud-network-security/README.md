# Cloud Network Security

This architecture diagram illustrates common components and patterns for securing networks within a cloud environment, covering edge protection, network segmentation, and security monitoring.

## Use Case

Designing and implementing a secure network infrastructure in the cloud to protect applications and data from external threats and control internal traffic flow.

## Architecture Overview

Cloud network security involves multiple layers of defense. This diagram shows how services at the global edge (DDoS protection, WAF, CDN) protect against common web exploits and volumetric attacks. Within the regional network (VPC/VNet), network segmentation using subnets, Network ACLs, and Security Groups/Network Security Groups controls traffic flow. Dedicated network firewalls provide more advanced traffic inspection and filtering. Hybrid connectivity options like VPN or Direct Connect/ExpressRoute are secured at the network edge. Security services like IDS/IPS, traffic mirroring, and flow logs provide visibility and threat detection, feeding into a centralized security posture management system.

## Key Components

### External Network
- **Internet**: The public network from which external traffic originates.

### Cloud Provider Network
- **Global Edge**: Services deployed at the cloud provider's edge locations to protect against threats before they reach the main network.
    - **DDoS Protection**: Services that mitigate Distributed Denial of Service attacks (e.g., AWS Shield, Azure Front Door, Google Cloud Armor).
    - **Web Application Firewall (WAF)**: Filters and monitors HTTP traffic between a web application and the Internet to protect against common web exploits (e.g., AWS WAF, Azure WAF, Google Cloud Armor).
    - **CDN (Caching, Edge Security)**: Content Delivery Networks can provide caching for performance and also offer edge security features.
- **Regional Network**: The core network within a specific cloud region.
    - **Virtual Private Cloud (VPC/VNet)**: An isolated network within the cloud provider's infrastructure where you launch your resources.
    - **Subnets**: Divisions within a VPC/VNet, often categorized as public (accessible from the internet) or private (internal only).
    - **Route Tables**: Control the traffic routing within the VPC/VNet and to/from external networks.
    - **Network ACLs (Stateless Filtering)**: Optional layer of security for your VPC/VNet that acts as a firewall for controlling traffic in and out of one or more subnets. They are stateless, meaning rules are applied independently to inbound and outbound traffic.
    - **Security Groups (Stateful Filtering)**: Act as a virtual firewall for your instances to control inbound and outbound traffic. They are stateful, automatically allowing return traffic.
    - **Network Firewall**: A managed service providing advanced network threat protection for your VPCs (e.g., AWS Network Firewall, Azure Firewall, Google Cloud Firewall).
    - **VPN / Direct Connect (Hybrid Connectivity)**: Secure connections between your on-premises network and the cloud VPC/VNet.
- **Security Services**: Tools for monitoring, detection, and analysis of network traffic and security events.
    - **Intrusion Detection/Prevention (IDS/IPS)**: Systems that monitor network traffic for malicious activity and can optionally block detected threats.
    - **Traffic Mirroring**: Copies network traffic from an elastic network interface to a destination for inspection.
    - **VPC Flow Logs**: Capture information about the IP traffic going to and from network interfaces in your VPC/VNet.
    - **Security Posture Management**: Centralized services that aggregate security findings, provide threat detection, and help manage your security posture (e.g., AWS Security Hub, Azure Security Center/Defender for Cloud, Google Cloud Security Command Center).

## Key Design Decisions

### Defense in Depth
- Implementing security controls at multiple layers (edge, network, host) provides a robust security posture.

### Network Segmentation
- Dividing the network into smaller segments (subnets) and controlling traffic between them limits the blast radius of a security breach.

### Stateful vs. Stateless Filtering
- Understanding and appropriately using Network ACLs (stateless) and Security Groups (stateful) for filtering traffic.

### Centralized Firewall Management
- Using a managed network firewall simplifies policy management and provides advanced threat protection.

### Visibility and Monitoring
- Implementing flow logs, traffic mirroring, and integrating with security posture management tools provides crucial visibility into network activity and potential threats.

### Secure Hybrid Connectivity
- Ensuring that connections between on-premises and cloud networks are encrypted and properly controlled.

## Real-World Applications

- Securing web applications and APIs
- Protecting internal corporate networks extended to the cloud
- Implementing compliance requirements (e.g., PCI-DSS, HIPAA)
- Isolating different environments (e.g., development, staging, production)
- Monitoring and responding to network security incidents
