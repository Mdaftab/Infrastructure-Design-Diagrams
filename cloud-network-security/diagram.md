# Cloud Network Security

```mermaid
graph LR
    subgraph "External Network"
        Internet["Internet"]
    end

    subgraph "Cloud Provider Network"
        subgraph "Global Edge"
            DDoSProtection["DDoS Protection (e.g., Shield, Front Door, Cloud Armor)"]
            WAF["Web Application Firewall (WAF)"]
            CDN["CDN (Caching, Edge Security)"]
        end

        subgraph "Regional Network"
            VPC["Virtual Private Cloud (VPC/VNet)"]
            Subnets["Subnets (Public/Private)"]
            RouteTables["Route Tables"]
            NetworkACLs["Network ACLs (Stateless Filtering)"]
            SecurityGroups["Security Groups (Stateful Filtering)"]
            Firewall["Network Firewall (e.g., Network Firewall, Azure Firewall, Cloud Firewall)"]
            VPN_DX["VPN / Direct Connect (Hybrid Connectivity)"]
        end

        subgraph "Security Services"
            IDS_IPS["Intrusion Detection/Prevention (IDS/IPS)"]
            TrafficMirroring["Traffic Mirroring"]
            FlowLogs["VPC Flow Logs"]
            SecurityHub["Security Posture Management (e.g., Security Hub, Defender, Security Command Center)"]
        end
    end

    Internet --> DDoSProtection
    DDoSProtection --> WAF
    WAF --> CDN
    WAF --> VPC

    CDN --> VPC

    VPC --> Subnets
    Subnets --> NetworkACLs
    Subnets --> SecurityGroups
    VPC --> RouteTables
    VPC --> Firewall
    VPC <--> VPN_DX

    NetworkACLs --> Subnets
    SecurityGroups --> Subnets

    Firewall --> Subnets

    Subnets --> IDS_IPS
    Subnets --> TrafficMirroring
    Subnets --> FlowLogs

    IDS_IPS --> SecurityHub
    TrafficMirroring --> SecurityHub
    FlowLogs --> SecurityHub
