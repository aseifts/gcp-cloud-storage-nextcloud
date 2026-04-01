# gcp-cloud-storage-nextcloud
A cloud-native Nextcloud deployment on GCP utilizing GCS (Object Storage) for decoupled data persistence, Caddy for automated SSL, and Identity-Aware Proxy (IAP) for secure administrative access.

```mermaid
graph TD
    A["User (Public Internet)"] --> B["Identity-Aware Proxy"]
    B --> C["VPC Firewall (Strict Deny)"]
    C --> D["Caddy (SSL/Reverse Proxy)"]
    D --> E["Nextcloud (Application)"]
    E --> F["MariaDB (Database)"]
    E --> G["Redis (Cache)"]
    E -.-> H["GCS Bucket (Object Storage)"]
