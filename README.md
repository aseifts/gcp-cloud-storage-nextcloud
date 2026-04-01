# gcp-cloud-storage-nextcloud
A cloud-native Nextcloud deployment on GCP utilizing GCS (Object Storage) for decoupled data persistence, Caddy for automated SSL, and Identity-Aware Proxy (IAP) for secure administrative access.

```mermaid
graph LR
    User --> IAP
    IAP --> Firewall
    Firewall --> Caddy
    Caddy --> Nextcloud
    Nextcloud --> MariaDB
    Nextcloud --> Redis
    Nextcloud -.-> GCS_Bucket
