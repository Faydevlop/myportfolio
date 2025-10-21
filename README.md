# WhatsApp Coexistence Backend Service

A comprehensive microservices-based backend system that enables multiple agents to coexist and manage WhatsApp Business API communications through a unified platform. This system provides real-time messaging, contact management, and agent coordination for organizations using WhatsApp Business.

## Project Overview

The WhatsApp Coexistence Backend Service is designed to solve the challenge of multiple agents working with a single WhatsApp Business number. It provides:

- **Multi-Agent Support**: Multiple agents can work simultaneously with the same WhatsApp Business number
- **Real-time Communication**: MQTT-based real-time messaging and status updates
- **Contact Management**: Centralized contact synchronization and management
- **Message History**: Complete message history tracking and retrieval
- **Agent Coordination**: Real-time agent activity tracking and status management
- **Push Notifications**: Mobile push notifications for offline agents

## Architecture

### Core Components

1. **Main Backend Service** (`/src`)
   - Express.js REST API server
   - Authentication and authorization
   - User and agent management
   - WhatsApp Business API integration
   - Database operations and business logic

2. **Inject Service** (`/inject-service`)
   - MQTT message injection service
   - Real-time message processing
   - Message routing and distribution

3. **Message Service** (`/message-service`)
   - Dedicated message processing service
   - Message status tracking
   - Redis-based caching and queuing

### Technology Stack

- **Backend**: Node.js, Express.js, TypeScript
- **Database**: MongoDB (Primary), Redis (Caching)
- **Real-time**: MQTT (EMQX Broker)
- **Authentication**: JWT with role-based access control
- **Cloud Storage**: AWS S3
- **Push Notifications**: Expo Push Notifications
- **Message Queue**: Redis, AMQP
- **API Documentation**: Swagger/OpenAPI

### Data Flow

```
WhatsApp Business API → Webhook → Backend Service → MQTT → Agent Clients
                                    ↓
                              Database (MongoDB)
                                    ↓
                              Redis Cache
                                    ↓
                              Push Notifications
```

## Key Features

### For Organizations
- **Multi-Agent Management**: Add and manage multiple agents for a single WhatsApp Business number
- **Real-time Monitoring**: Track agent activity and message status in real-time
- **Contact Synchronization**: Automatic contact sync from WhatsApp Business
- **Message History**: Complete conversation history with search and filtering
- **Analytics**: Message status tracking and delivery reports

### For Agents
- **Real-time Messaging**: Send and receive messages in real-time
- **Contact Management**: View and manage contacts with conversation history
- **Status Tracking**: See message delivery status and read receipts
- **Push Notifications**: Get notified of new messages when offline
- **Multi-device Support**: Work from multiple devices simultaneously

### Technical Features
- **Scalable Architecture**: Microservices-based design for horizontal scaling
- **Real-time Updates**: MQTT-based real-time communication
- **Caching**: Redis-based caching for improved performance
- **Security**: JWT authentication with role-based access control
- **Monitoring**: Comprehensive logging and error handling
- **API Documentation**: Auto-generated Swagger documentation

## Project Structure

```
whatsapp-coexistence-bk-service/
├── src/                          # Main backend service
│   ├── controllers/              # API controllers
│   ├── services/                 # Business logic services
│   ├── db/                       # Database models and connections
│   ├── middlewares/              # Express middlewares
│   ├── routes/                   # API routes
│   └── utils/                    # Utility functions
├── inject-service/               # MQTT injection service
├── message-service/              # Message processing service
├── dist/                         # Compiled executables
└── public/                       # Static files and documentation
```

## System Components

### Main Backend Service
The primary REST API server that handles:
- User authentication and authorization
- Agent and organization management
- WhatsApp Business API integration
- Database operations and business logic
- Real-time message processing coordination

### Inject Service
A specialized MQTT service responsible for:
- Message injection into the MQTT broker
- Real-time message routing and distribution
- Message queue management
- Integration with external messaging systems

### Message Service
A dedicated message processing service that handles:
- Message status tracking and updates
- Redis-based caching and queuing
- Message delivery confirmation
- Push notification triggers

## System Architecture

### Data Flow
```
WhatsApp Business API → Webhook → Backend Service → MQTT → Agent Clients
                                    ↓
                              Database (MongoDB)
                                    ↓
                              Redis Cache
                                    ↓
                              Push Notifications
```

### Technology Stack
- **Backend**: Node.js, Express.js, TypeScript
- **Database**: MongoDB (Primary), Redis (Caching)
- **Real-time**: MQTT (EMQX Broker)
- **Authentication**: JWT with role-based access control
- **Cloud Storage**: AWS S3
- **Push Notifications**: Expo Push Notifications
- **Message Queue**: Redis, AMQP

## Core Functionality

### Multi-Agent Coordination
- Multiple agents can work simultaneously with a single WhatsApp Business number
- Real-time agent activity tracking and status management
- Message distribution and routing to appropriate agents
- Conflict resolution for concurrent agent access

### Message Management
- Complete message history tracking and retrieval
- Real-time message synchronization across all connected agents
- Message status tracking (sent, delivered, read)
- Media file handling and storage

### Contact Management
- Automatic contact synchronization from WhatsApp Business
- Centralized contact database with search and filtering
- Contact metadata management
- Conversation history per contact

### Real-time Communication
- MQTT-based real-time messaging
- Live agent status updates
- Instant message delivery to online agents
- Push notifications for offline agents

## API Services

The system provides comprehensive REST APIs for:
- **Authentication Services**: OTP-based login, token management, session handling
- **User Management**: Agent creation, profile management, role assignment
- **Contact Services**: Contact synchronization, search, and management
- **Message Services**: Send messages, retrieve conversation history, status tracking
- **Organization Services**: Business phone management, configuration
- **Webhook Integration**: WhatsApp Business API webhook processing

## Security Implementation

- JWT-based authentication with refresh token mechanism
- Role-based access control (Admin/Agent permissions)
- Rate limiting and request validation
- Secure file upload and media handling
- CORS configuration for cross-origin requests
- Security headers implementation

## Mobile Application Support

- Expo push notification integration for mobile apps
- Real-time message synchronization across devices
- Offline message queuing and delivery
- Multi-device session management
- Cross-platform compatibility

---

**System Requirements**: This backend service requires a frontend application (web or mobile) to provide the complete user experience. The system is designed to work with WhatsApp Business API and requires proper WhatsApp Business account configuration.
