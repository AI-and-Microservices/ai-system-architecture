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
    Traefik --> | Webhook api | IntegrationMessagePlatform
    Traefik --> AppicationService

    AppicationService --> |New conversation| ConversationService
    IntegrationMessagePlatform --> |New message| ConversationService
    SocketService

    Nginx --> DashboardFE
    FileService --> Minio
    UserService --> [(MongoDB)]
    SocketService --> Redis
    SocketService --> |Direct incoming message| Kafka
    AppicationService --> MongoDB
    Kafka --> FunctionCallService
    ChatbotService --> |Function call data| Kafka
    IntegrationMessagePlatform --> |Incomming message| Kafka
    Kafka --> |Recept new message| ChatbotService
    ChatbotService -->|Out going message| Kafka
    Kafka --> |direct outgoing message| SocketService
    Kafka --> |Outgoing message| IntegrationMessagePlatform
```

### Key Components
- **Traefik Gateway**: Central entry point for all APIs and Frontend traffic.

- **MongoDB**: Stores user data and system metadata.

- **Redis**: Provides caching and pub/sub capabilities.

- **Kafka**: Event streaming for chatbot and realtime communication.

- **Minio**: Object storage for user files and attachments.

### Core Services

#### Service	Description:
File Service	Handles file uploads and storage management
Dashboard FE	Frontend application served via Nginx
User Service	Authentication, authorization, user data
Socket Service	Real-time WebSocket communication
Chatbot Service	Manages chatbot sessions and AI responses

#### Message Flow
The user interacts with the frontend (Dashboard FE) via browser.

Traefik routes API requests to the respective services.

Chatbot Service publishes events to Kafka after processing messages.

Socket Service listens to Kafka events and pushes realtime updates to the clients over WebSocket.

Deployment
The system is orchestrated using Docker Compose for local development.
For production deployment, Kubernetes or ECS is recommended.

