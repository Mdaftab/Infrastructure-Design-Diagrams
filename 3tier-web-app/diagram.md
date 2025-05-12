# 3-Tier Web Application

```mermaid
graph LR
    subgraph "Client Layer"
        User["End User"]
    end

    subgraph "Presentation Layer"
        LoadBalancer["Load Balancer\n(e.g., ALB, Nginx)"]
        WebServer["Web Servers\n(e.g., Apache, Nginx)"]
    end

    subgraph "Application Layer"
        AppServers["Application Servers\n(e.g., Node.js, Python, Java)"]
    end

    subgraph "Data Layer"
        Database["Database\n(e.g., PostgreSQL, MySQL)"]
        Cache["Cache\n(e.g., Redis, Memcached)"]
    end

    User --> LoadBalancer
    LoadBalancer --> WebServer
    WebServer --> AppServers
    AppServers --> Database
    AppServers --> Cache
