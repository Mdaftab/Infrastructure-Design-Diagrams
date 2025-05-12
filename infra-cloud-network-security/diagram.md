# Cloud Network Security

```mermaid
graph LR
    subgraph "External Network"
        Internet["Internet"]
    end

    subgraph "Cloud Provider Network"
        subgraph "Global Edge"
            DDoSProtection["DDoS Protection\n(e.g., Shield, Front Door, Cloud Armor)"]
            WAF["Web Application Firewall\n(WAF)"]
            CDN["CDN\n(Caching, Edge Security)"]
        end

        subgraph "Regional Network"
            VPC["Virtual Private Cloud\n(VPC/VNet)"]
            Subnets["Subnets\n(Public/Private)"]
            RouteTables["Route Tables"]
            NetworkACLs["Network ACLs\n(Stateless Filtering)"]
            SecurityGroups["Security Groups\n(Stateful Filtering)"]
            Firewall["Network Firewall\n(e.g., Network Firewall, Azure Firewall, Cloud Firewall)"]
            VPN_DX["VPN / Direct Connect\n(Hybrid Connectivity)"]
        end

        subgraph "Security Services"
            IDS_IPS["Intrusion Detection/Prevention\n(IDS/IPS)"]
            TrafficMirroring["Traffic Mirroring"]
            FlowLogs["VPC Flow Logs"]
            SecurityHub["Security Posture Management\n(e.g., Security Hub, Defender, Security Command Center)"]
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
