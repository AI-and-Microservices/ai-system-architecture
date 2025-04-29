# System Architecture Overview

## High Level Overview

### Base flow
```mermaid
flowchart TD
    User --> Traefik
    Traefik --> Nginx
    Traefik --> FileService
    Traefik --> UserService
    Traefik --> SocketService
    Traefik --> ChatbotService
    Traefik --> | Webhook api | IntegrationMessagePlatform
    Traefik --> AppicationService
    Traefik --> FunctionCallService
    Traefik --> ConversationService

    Nginx --> DashboardFE
    FileService --> Minio
    UserService --> MongoDB
    UserService --> Redis
    SocketService --> Redis
    SocketService --> |Direct incoming message| Kafka
    AppicationService --> MongoDB
    
    IntegrationMessagePlatform --> |Incomming message| Kafka
    Kafka <--> AIAgent
    Kafka <--> |Message processing| ConversationService

```

### Message flow
```mermaid
flowchart TD
    Traefik --> SocketService
    Traefik --> | Webhook api | IntegrationMessagePlatform
    IntegrationMessagePlatform --> |New message| ConversationService
    SocketService --> |New message| ConversationService
    ConversationService --> |New incomming message| Kafka
    Kafka --> |New message| AIAgent
    AIAgent <--> |get app config| AppicationService
    AIAgent <--> |get chatbot prompt| ChatbotService
    AIAgent <--> |Get message history| ConversationService
    AIAgent --> |Call AI API| id1(OpenAI/Gemini/grok/deepseek)
    AIAgent --> FunctionCallService
    AIAgent --> |Outgoing message| Kafka
    
    FunctionCallService --> |API Call| 3rdparty
    FunctionCallService --> |Save data| id2[(MongoDB)]
    FunctionCallService --> |Save function_call result| ConversationService
    Kafka --> |Save message| ConversationService
    
  
  
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

