# AI Agent Microservices System

This project demonstrates the architecture of a scalable AI agent system using a microservices approach.  
It provides infrastructure components and a set of services designed for modular, resilient AI-driven applications.

## Features
- Modular Microservices Architecture
- Real-time Communication via WebSocket
- AI Agent Management
- Scalable File Management System
- Message-Driven Architecture (Kafka)

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
- **Chatbot Service**: Chatbot and AI Agent Logic

## Quick Start
You can spin up the full system using Docker Compose:

```bash
docker-compose up --build
