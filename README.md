# gcp-cloud-storage-nextcloud
A cloud-native Nextcloud deployment on GCP utilizing GCS (Object Storage) for decoupled data persistence, Caddy for automated SSL, and Identity-Aware Proxy (IAP) for secure administrative access.

```mermaid
graph TD
    %% User and Internet
    User["User on Public Internet"] -->|Resolves Domain| DNS["GCP Cloud DNS/Public DNS"]
    DNS -->|"Traffic (Port 443)"| IAP

    %% GCP Security Layer
    subgraph GCP_Security ["GCP External Security Perimeter"]
        IAP("Identity-Aware Proxy <br/> Authentication Barrier")
    end
    IAP -->|"Authenticated Traffic"| LB["Optional Load Balancer <br/> Or Public IP"]

    %% VPC Networking Layer
    subgraph GCP_VPC ["Custom VPC Network"]
        LB -->|Traffic| FW("Strict VPC Firewall Rules <br/> Default Deny, Allow IAP/HTTP/HTTPS")
    end

    %% Compute Layer (Docker Compose Stack)
    subgraph GCE_Instance ["GCE Instance - Container-Optimized OS"]
        subgraph Docker_Compose ["Docker Compose Multi-Tier Stack"]
            direction TB
            FW -->|"Encrypted Traffic (443)"| Caddy("Caddy Reverse Proxy <br/> SSL Termination & Header Hardening")
            Caddy -->|"Internal Proxy"| Nextcloud("Nextcloud Application")
            Nextcloud <-->|"Internal Network"| MariaDB("MariaDB Database")
            Nextcloud <-->|"Internal Network"| Redis("Redis Cache")
        end
    end

    %% Storage Layer
    subgraph Storage_Layer ["Decoupled Storage Backend"]
        Nextcloud -.->|"IAM Service Account Access"| GCS("GCS Object Storage Bucket <br/> Private Data Persistence")
    end

    %% Connect Nextcloud to GCS
    linkStyle 10 stroke-width:2px,stroke-dasharray: 5 5,stroke:red;
