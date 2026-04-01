# gcp-cloud-storage-nextcloud
A cloud-native Nextcloud deployment on GCP utilizing GCS (Object Storage) for decoupled data persistence, Caddy for automated SSL, and Identity-Aware Proxy (IAP) for secure administrative access.

```mermaid
graph TD
    %% User and Internet
    User["User on Public Internet"] --> DNS["GCP Cloud DNS"]
    DNS -->|"Traffic (443)"| IAP["Identity-Aware Proxy"]

    %% Security & Networking
    subgraph Network ["GCP Security & VPC"]
        IAP --> FW["VPC Firewall (Strict Deny)"]
    end

    %% Compute Stack
    subgraph GCE ["GCE Instance (Parrot OS/COS)"]
        FW --> Caddy["Caddy (SSL/Headers)"]
        Caddy --> Nextcloud["Nextcloud App"]
        Nextcloud <--> DB["MariaDB"]
        Nextcloud <--> Redis["Redis"]
    end

    %% Storage
    subgraph Storage ["Cloud Storage"]
        Nextcloud -.->|"IAM Auth"| GCS["GCS Bucket"]
    end
