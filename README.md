# CST8915-Miterm-Project
## 🎯 Overview

The Algonquin Pet Store is a full-stack microservices application that demonstrates:
- **Microservices Architecture**: Independently deployable services
- **Message-Driven Communication**: Asynchronous processing via RabbitMQ
- **Cloud-Native Deployment**: Fully deployed on Microsoft Azure
- **Real-Time Analytics**: Order processing and business intelligence
- **12-Factor App Compliance**: Industry best practices

## Features
- Consumes orders from RabbitMQ queue
- REST API for order analytics
- Real-time order processing
- 12-factor app compliant

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check |
| GET | `/orders` | List all orders |
| GET | `/analytics/summary` | Total orders, revenue, average |
| GET | `/analytics/products` | Orders by product |
| GET | `/analytics/top-products` | Top 3 products by quantity |

## 🚀 Services

| Service | Technology | Azure Service | Port | Purpose | Status |
|---------|------------|---------------|------|---------|--------|
| **store-front** | Vue.js | Static Web Apps | 80/443 | Customer-facing web application | ✅ Deployed |
| **order-service** | Node.js/Express | App Service | 3000 | Processes orders, publishes to RabbitMQ | ✅ Deployed |
| **product-service** | Python/Flask | App Service | 3030 | Serves product catalog via REST API | ✅ Deployed |
| **RabbitMQ** | Erlang/RabbitMQ | Azure VM (Ubuntu) | 5672,15672 | Message broker for async communication | ✅ Deployed |
| **order-analytics-service** | Node.js/Express | App Service | 4000 | Consumes orders, provides analytics API | 🆕 New |

## 📋 Prerequisites

### Required Accounts & Tools
- [ ] **Azure Subscription** (with contributor access)
- [ ] **GitHub Account** (for CI/CD)
- [ ] **Node.js** (v18 or higher)
- [ ] **Python** (v3.9 or higher)
- [ ] **Azure CLI** (latest version)
- [ ] **Git** (for version control)

## 12-Factor App Compliance

| Factor | Implementation |
|--------|---------------|
| I. Codebase | Separate GitHub repository |
| II. Dependencies | All dependencies in package.json |
| III. Config | Environment variables via .env |
| IV. Backing Services | RabbitMQ connection string configurable |


## RabbitMQ Consumer Pattern
```text

┌─────────────────┐     ┌─────────────────────────────────────┐
│   order-service │     │      RabbitMQ Broker                 │
│   (Producer)    │────▶│  ┌────────────────┐                 │
└─────────────────┘     │  │  order_queue   │                 │
                        │  └────────────────┘                 │
                        │          │                           │
                        │          │ Push delivery             │
                        │          ▼                           │
                        │  ┌────────────────┐                 │
                        │  │ Consumer (Push)│                 │
                        │  │ with prefetch  │                 │
                        │  └────────────────┘                 │
                        │          │                           │
                        └──────────┼──────────────────────────┘
                                   │
                                   ▼
                        ┌─────────────────────┐
                        │ order-analytics     │
                        │ service (Consumer)  │
                        └─────────────────────┘
```



 Why prefetch matters: Without prefetch, RabbitMQ could overwhelm your consumer by pushing thousands of messages, 
 leading to memory issues. Prefetch creates a "window" of in-flight message

 Justification for Push Pattern Selection
 
## Alignment with RabbitMQ's Design Philosophy
RabbitMQ is fundamentally designed as a push-based broker . The AMQP protocol, which RabbitMQ implements, specifies that the broker is responsible for "pushing" messages to consumers. Using push mode leverages RabbitMQ's core strengths.

## Real-time Analytics Requirement
The Order Analytics Service needs to provide up-to-date analytics. Push mode ensures orders are processed milliseconds after being placed, enabling real-time dashboard updates .

                        

