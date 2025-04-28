
# System Architecture Overview

## High Level Overview

```mermaid
flowchart TB
    User --> Traefik
    Traefik --> Nginx
    Traefik --> FileService
    Traefik --> UserService
    Traefik --> SocketService
    Traefik --> ChatbotService

    Nginx --> DashboardFE
    FileService --> Minio
    UserService --> MongoDB
    SocketService --> Redis
    ChatbotService -->|publish events| Kafka
    SocketService -->|subscribe events| Kafka
```
