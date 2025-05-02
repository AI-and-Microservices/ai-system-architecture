# AI Agent Microservices System

This project demonstrates the architecture of a scalable AI agent system using a microservices approach.  
It provides infrastructure components and a set of services designed for modular, resilient AI-driven applications.

## Features
- Modular Microservices Architecture
- AI Agent Management
- Message-Driven Architecture (Kafka)
- Modular Virtual Roles: Each virtual role in the system has a dedicated prompt and responsibility—think of them like AI microservices, each handling a specific job.
- Flexible App Logic: Apps can use one or multiple virtual roles in sequence (multi-step logic) or rotate between them (round-robin style), depending on the use case.
- Function Call Support: Easily integrate function calls such as database operations, third-party API calls, or even sending notifications to Slack, Messenger, or WhatsApp—without touching the core code.
- RAG Integration: Improve context-awareness and knowledge grounding by retrieving relevant documents before generating responses.
- Prompt Engineering Guide: A built-in guide for writing effective prompts tailored for different roles—perfect for those new to LLM development.

## Infrastructure
- MongoDB: Database
- Redis: Caching and Pub/Sub
- Minio: Object Storage (S3-compatible)
- Kafka: Message Broker
- Traefik: API Gateway and Load Balancer
- Nginx: Frontend Server

## Services
- **File Service**: Manage and upload files
- **Dashboard Frontend Service**: Web Interface
- **User Service**: User Authentication and Management
- **Socket Service**: Realtime Messaging (WebSocket)
- **Virtual role Service**: Chatbot and AI Agent Logic

## Quick Start
You can spin up the full system using Docker Compose:

```bash
docker-compose up --build
