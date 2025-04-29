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
    %% ENTRY
    subgraph Entry ["Client & Input"]
        Traefik --> SocketService:::entry
        Traefik -->|Webhook API| IntegrationMessagePlatform:::entry
        IntegrationMessagePlatform -->|New message| Kafka_incoming:::kafka
        SocketService -->|New message| Kafka_incoming:::kafka
    end

    %% CORE PROCESSING
    subgraph CoreProcessing ["MessageProcessor"]
        Kafka_incoming --> MessageProcessor:::processing
        MessageProcessor -->|Call AI API| AIService(OpenAI/Gemini/grok):::processing
        MessageProcessor -->|get config| AppicationService:::processing
        MessageProcessor -->|get prompt| ChatbotService:::processing
        MessageProcessor -->|get history| ConversationService:::persistence
        MessageProcessor --> Kafka_outgoing:::kafka
        MessageProcessor --> Kafka_function_call:::kafka
    end

    %% FUNCTION CALL
    subgraph FunctionCall ["Function Dispatcher"]
        Kafka_function_call --> FunctionDispatcher:::function
        FunctionDispatcher -->|API call| 3rdparty:::function
        FunctionDispatcher -->|Save result| ConversationService:::persistence
        FunctionDispatcher -->|Save to DB| MongoDB:::persistence
    end

    %% OUTGOING
    subgraph Persistence ["Outgoing & Storage"]
        Kafka_outgoing --> ConversationService:::persistence
    end

    %% CLASS DEFINITIONS
    classDef entry fill:#e1fbe1,stroke:#34c759,color:#000;
    classDef processing fill:#e0f0ff,stroke:#007aff,color:#000;
    classDef kafka fill:#ffe9d6,stroke:#ff9500,color:#000;
    classDef function fill:#f0e7ff,stroke:#af52de,color:#000;
    classDef persistence fill:#f2f2f7,stroke:#8e8e93,color:#000;
  
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

