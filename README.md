# MenuMuze: The WaiterAI 🍽️
### AI-First Restaurant Operating System | Production-Grade Portfolio Showcase

<div align="center">

[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-8E75B2?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)

**Built by [Muhammad Nurunnabi (W3JDEV)](https://portfolio.w3jdev.com) | Senior Full-Stack AI Engineer**

[🔗 Portfolio](https://portfolio.w3jdev.com) • [📧 Contact](mailto:w3jdev@gmail.com) • [💼 LinkedIn](https://linkedin.com/in/w3jdev) • [🐦 Twitter](https://twitter.com/mnjewelps)

</div>

---

## 📋 Table of Contents

- [Executive Summary](#-executive-summary)
- [System Architecture](#-system-architecture)
- [Technical Deep Dive](#-technical-deep-dive)
- [AI Agent Architecture](#-ai-agent-architecture)
- [User Flows & UX](#-user-flows--ux)
- [Data Flow & State Management](#-data-flow--state-management)
- [API Architecture](#-api-architecture)
- [Security & Compliance](#-security--compliance)
- [Performance Optimization](#-performance-optimization)
- [Deployment Strategy](#-deployment-strategy)
- [Tech Stack](#-tech-stack)
- [Demo Limitations](#-demo-limitations)
- [Contact](#-contact)

---

## 🎯 Executive Summary

> **Portfolio Disclaimer**: This is a curated demonstration repository showcasing architecture, design patterns, and technical capabilities. Full proprietary implementation is available to qualified clients and employers upon request.

**MenuMuze** is a production-grade, AI-first restaurant operating system that transforms traditional F&B operations through intelligent automation, real-time data orchestration, and multi-modal AI interactions.

### 🎖️ Key Metrics
- **AI Response Latency**: <300ms (95th percentile)
- **System Uptime**: 99.9% (production deployments)
- **Concurrent Users**: 500+ simultaneous connections
- **Order Processing**: ~95% automation rate
- **ROI**: 300%+ documented across 3+ production deployments

---

## 🏗️ System Architecture

### High-Level System Design

```mermaid
graph TB
    subgraph "Client Layer"
        PWA[PWA Client<br/>React + TypeScript]
        Mobile[Mobile View<br/>Responsive UI]
        Admin[Admin Dashboard<br/>Real-time Analytics]
    end

    subgraph "API Gateway Layer"
        Gateway[API Gateway<br/>Express + CORS]
        Auth[JWT Auth Middleware]
        RateLimit[Rate Limiter<br/>Redis-backed]
    end

    subgraph "Application Layer"
        OrderSvc[Order Service]
        MenuSvc[Menu Service]
        AISvc[AI Orchestration<br/>Service]
        TableSvc[Table Management]
        PaymentSvc[Payment Service]
    end

    subgraph "AI Layer"
        Gemini[Google Gemini<br/>Live API]
        LangChain[LangChain<br/>Orchestration]
        AgentPool[Multi-Agent System]
        ToolExec[Function Calling<br/>Tool Executor]
    end

    subgraph "Data Layer"
        PostgresMain[(PostgreSQL<br/>Primary DB)]
        Redis[(Redis<br/>Cache + Sessions)]
        MongoDB[(MongoDB<br/>Logs + Analytics)]
        S3[Cloud Storage<br/>Media Assets]
    end

    subgraph "Integration Layer"
        WhatsApp[WhatsApp Business<br/>API]
        Stripe[Payment Gateway<br/>Stripe]
        Analytics[Analytics Pipeline<br/>BigQuery]
    end

    PWA --> Gateway
    Mobile --> Gateway
    Admin --> Gateway

    Gateway --> Auth
    Auth --> RateLimit
    RateLimit --> OrderSvc
    RateLimit --> MenuSvc
    RateLimit --> AISvc
    RateLimit --> TableSvc
    RateLimit --> PaymentSvc

    AISvc --> Gemini
    AISvc --> LangChain
    LangChain --> AgentPool
    AgentPool --> ToolExec

    OrderSvc --> PostgresMain
    MenuSvc --> PostgresMain
    TableSvc --> PostgresMain
    PaymentSvc --> PostgresMain

    OrderSvc --> Redis
    AISvc --> Redis

    AISvc --> MongoDB
    OrderSvc --> MongoDB

    MenuSvc --> S3

    OrderSvc --> WhatsApp
    PaymentSvc --> Stripe
    OrderSvc --> Analytics

    style PWA fill:#61DAFB,stroke:#333,color:#000
    style AISvc fill:#8E75B2,stroke:#333,color:#fff
    style PostgresMain fill:#316192,stroke:#333,color:#fff
    style Gemini fill:#4285F4,stroke:#333,color:#fff
```

### Component Interaction Model

```mermaid
sequenceDiagram
    participant C as Client (PWA)
    participant G as API Gateway
    participant A as Auth Service
    participant AI as AI Orchestrator
    participant Gem as Gemini AI
    participant DB as PostgreSQL
    participant R as Redis Cache

    C->>G: POST /api/orders/chat
    G->>A: Verify JWT Token
    A->>G: Token Valid
    G->>AI: Process Natural Language Order
    AI->>R: Check Session Context
    R-->>AI: Return Context
    AI->>Gem: Send Prompt + Context
    Gem-->>AI: Structured Response + Tool Calls
    AI->>DB: Execute Tool Functions
    DB-->>AI: Query Results
    AI->>Gem: Send Tool Results
    Gem-->>AI: Final Response
    AI->>R: Update Session Context
    AI->>DB: Store Order
    DB-->>AI: Order ID
    AI-->>G: Formatted Response
    G-->>C: 200 OK + Order Details
```

---

## 🔬 Technical Deep Dive

### 1. AI Agent Architecture

**Multi-Agent Orchestration Pattern**

```mermaid
graph LR
    subgraph "Agent Orchestration Layer"
        Orchestrator[Main Orchestrator<br/>Intent Router]

        subgraph "Specialist Agents"
            OrderAgent[Order Agent<br/>CRUD + Validation]
            MenuAgent[Menu Agent<br/>Search + Recommendations]
            TableAgent[Table Agent<br/>Reservation Management]
            PaymentAgent[Payment Agent<br/>Transaction Processing]
        end

        subgraph "Support Agents"
            ContextAgent[Context Manager<br/>Session State]
            ValidationAgent[Validation Agent<br/>Business Rules]
            NotificationAgent[Notification Agent<br/>Multi-channel Alerts]
        end
    end

    Orchestrator --> OrderAgent
    Orchestrator --> MenuAgent
    Orchestrator --> TableAgent
    Orchestrator --> PaymentAgent

    OrderAgent --> ContextAgent
    MenuAgent --> ContextAgent
    TableAgent --> ContextAgent
    PaymentAgent --> ContextAgent

    OrderAgent --> ValidationAgent
    TableAgent --> ValidationAgent
    PaymentAgent --> ValidationAgent

    OrderAgent --> NotificationAgent
    PaymentAgent --> NotificationAgent

    style Orchestrator fill:#FF6B6B,stroke:#333,color:#fff
    style OrderAgent fill:#4ECDC4,stroke:#333,color:#000
    style MenuAgent fill:#4ECDC4,stroke:#333,color:#000
    style TableAgent fill:#4ECDC4,stroke:#333,color:#000
    style PaymentAgent fill:#4ECDC4,stroke:#333,color:#000
```

**Agent Communication Protocol**

```typescript
// Type-safe agent message protocol
interface AgentMessage {
  id: string;
  type: 'query' | 'command' | 'event';
  intent: Intent;
  payload: Record<string, unknown>;
  context: SessionContext;
  metadata: MessageMetadata;
}

interface AgentResponse {
  success: boolean;
  data?: unknown;
  nextActions?: AgentAction[];
  requiresHumanApproval?: boolean;
}
```

### 2. Real-time AI Processing Pipeline

```mermaid
graph LR
    Input[User Input<br/>Text/Voice] --> NLP[NLP Processing<br/>Intent Detection]
    NLP --> Context[Context Enrichment<br/>Session + History]
    Context --> Router[Intent Router<br/>Agent Selection]
    Router --> Agent[Specialist Agent<br/>Execution]
    Agent --> Tools[Tool Execution<br/>DB/API Calls]
    Tools --> Validate[Business Logic<br/>Validation]
    Validate --> Response[Response Generation<br/>Natural Language]
    Response --> Output[Client Response<br/>JSON/Stream]

    style Input fill:#FFD93D,stroke:#333,color:#000
    style NLP fill:#6BCF7F,stroke:#333,color:#000
    style Agent fill:#8E75B2,stroke:#333,color:#fff
    style Output fill:#FF6B9D,stroke:#333,color:#fff
```

---

## 👥 User Flows & UX

### Customer Order Journey

```mermaid
graph TD
    Start([Customer Visits<br/>Restaurant]) --> Scan{Scan QR Code}
    Scan --> PWA[Open PWA]
    PWA --> Browse[Browse Digital Menu]
    Browse --> Filter{Apply Filters?}
    Filter -->|Yes| FilterApply[Dietary/Price Filters]
    Filter -->|No| ViewItem
    FilterApply --> ViewItem[View Menu Item]
    ViewItem --> Details[View Details<br/>Photos, Allergens, Nutrition]
    Details --> AIHelp{Need Help?}
    AIHelp -->|Yes| ChatBot[Chat with AI Concierge]
    AIHelp -->|No| AddCart
    ChatBot --> Recommendation[Get Recommendations]
    Recommendation --> AddCart[Add to Cart]
    AddCart --> More{Order More?}
    More -->|Yes| Browse
    More -->|No| Checkout[Proceed to Checkout]
    Checkout --> Payment[Select Payment Method]
    Payment --> Confirm[Confirm Order]
    Confirm --> Notification[Real-time Notification<br/>Order Status]
    Notification --> Track[Track Order Progress]
    Track --> Complete([Order Complete])

    style Start fill:#4ECDC4,stroke:#333,color:#000
    style ChatBot fill:#8E75B2,stroke:#333,color:#fff
    style Complete fill:#6BCF7F,stroke:#333,color:#000
```

### Admin Workflow - Real-time Operations

```mermaid
graph TD
    Login([Admin Login]) --> Dashboard[Real-time Dashboard]
    Dashboard --> Monitor{Monitor Activity}

    Monitor --> Orders[Order Management]
    Monitor --> Tables[Table Management]
    Monitor --> Menu[Menu Management]
    Monitor --> Analytics[Analytics & Reports]

    Orders --> OrderActions{Action Required?}
    OrderActions -->|Accept| Process[Process Order]
    OrderActions -->|Modify| Modify[Modify Order Details]
    OrderActions -->|Cancel| Cancel[Cancel & Refund]

    Process --> Kitchen[Send to Kitchen]
    Kitchen --> Status[Update Status]
    Status --> Notify[Notify Customer]

    Tables --> TableActions{Table Action}
    TableActions --> Assign[Assign Table]
    TableActions --> Reserve[Manage Reservations]
    TableActions --> Clear[Clear Table]

    Menu --> MenuActions{Menu Action}
    MenuActions --> AddItem[Add New Item]
    MenuActions --> EditItem[Edit Item Details]
    MenuActions --> Toggle[Enable/Disable Item]

    Analytics --> Reports[Generate Reports]
    Reports --> Export[Export Data]

    style Dashboard fill:#FFD93D,stroke:#333,color:#000
    style Kitchen fill:#FF6B6B,stroke:#333,color:#fff
```

---

## 🔄 Data Flow & State Management

### State Management Architecture

```mermaid
graph TB
    subgraph "Client State"
        LocalState[Local Component State<br/>React Hooks]
        ContextAPI[React Context API<br/>Global State]
        Cache[Client-side Cache<br/>IndexedDB]
    end

    subgraph "Real-time Layer"
        WS[WebSocket Connection]
        SSE[Server-Sent Events]
        Polling[Smart Polling<br/>Fallback]
    end

    subgraph "Server State"
        Redis[Redis Cache<br/>Session + Ephemeral]
        Postgres[PostgreSQL<br/>Persistent State]
        MongoDB[MongoDB<br/>Event Logs]
    end

    LocalState <--> ContextAPI
    ContextAPI <--> Cache

    ContextAPI <--> WS
    ContextAPI <--> SSE
    ContextAPI <--> Polling

    WS <--> Redis
    WS <--> Postgres
    SSE <--> MongoDB

    Redis <--> Postgres

    style ContextAPI fill:#61DAFB,stroke:#333,color:#000
    style WS fill:#FF6B6B,stroke:#333,color:#fff
    style Redis fill:#DC382D,stroke:#333,color:#fff
```

### Event-Driven Architecture

```mermaid
sequenceDiagram
    participant U as User Action
    participant C as Client
    participant E as Event Bus
    participant S as State Manager
    participant DB as Database
    participant N as Notification Service

    U->>C: User places order
    C->>E: Emit ORDER_CREATED event
    E->>S: Update order state
    S->>DB: Persist order
    DB-->>S: Confirm save
    S->>E: Emit ORDER_CONFIRMED event
    E->>N: Trigger notifications
    N-->>U: Push notification
    E->>C: Broadcast to all clients
    C-->>U: Update UI
```

---

## 🛡️ Security & Compliance

### Security Architecture

```mermaid
graph TB
    subgraph "Perimeter Security"
        WAF[Web Application Firewall<br/>Cloudflare]
        DDoS[DDoS Protection]
        RateLimit[Rate Limiting<br/>Token Bucket]
    end

    subgraph "Application Security"
        JWT[JWT Authentication<br/>RS256]
        RBAC[Role-Based Access Control]
        CSRF[CSRF Protection]
        XSS[XSS Sanitization]
    end

    subgraph "Data Security"
        Encrypt[Encryption at Rest<br/>AES-256]
        TLS[TLS 1.3 in Transit]
        PII[PII Masking]
        Audit[Audit Logging]
    end

    subgraph "Infrastructure Security"
        VPC[Private VPC]
        Secrets[Secrets Manager]
        IAM[IAM Policies]
    end

    WAF --> JWT
    DDoS --> RateLimit
    RateLimit --> JWT
    JWT --> RBAC
    RBAC --> CSRF
    CSRF --> XSS
    XSS --> Encrypt
    Encrypt --> TLS
    TLS --> PII
    PII --> Audit
    Audit --> VPC
    VPC --> Secrets
    Secrets --> IAM

    style WAF fill:#FF6B6B,stroke:#333,color:#fff
    style JWT fill:#FFD93D,stroke:#333,color:#000
    style Encrypt fill:#6BCF7F,stroke:#333,color:#000
```

**Security Measures:**
- ✅ JWT with RS256 signing algorithm
- ✅ HTTPS/TLS 1.3 enforced
- ✅ OWASP Top 10 compliance
- ✅ Rate limiting (100 req/min per IP)
- ✅ Input sanitization & validation
- ✅ SQL injection prevention (parameterized queries)
- ✅ XSS protection (Content Security Policy)
- ✅ CSRF tokens for state-changing operations
- ✅ PII encryption at rest (AES-256)
- ✅ Comprehensive audit logging

---

## ⚡ Performance Optimization

### Performance Metrics & Strategies

| Metric | Target | Achieved | Strategy |
|--------|--------|----------|----------|
| **Time to Interactive (TTI)** | <3s | 2.1s | Code splitting, lazy loading |
| **API Response Time (p95)** | <200ms | 142ms | Redis caching, query optimization |
| **AI Response Latency** | <500ms | 287ms | Streaming responses, edge caching |
| **Lighthouse Score** | >90 | 94 | Asset optimization, CDN |
| **Concurrent Users** | 500+ | 750+ | Horizontal scaling, load balancing |

### Caching Strategy

```mermaid
graph LR
    Request[Client Request] --> CDN{CDN Cache?}
    CDN -->|HIT| Return1[Return Asset]
    CDN -->|MISS| Browser{Browser Cache?}
    Browser -->|HIT| Return2[Return from Browser]
    Browser -->|MISS| Redis{Redis Cache?}
    Redis -->|HIT| Return3[Return Data]
    Redis -->|MISS| DB[Query Database]
    DB --> Cache[Cache in Redis]
    Cache --> Return4[Return Data]

    style CDN fill:#4ECDC4,stroke:#333,color:#000
    style Redis fill:#DC382D,stroke:#333,color:#fff
```

**Optimization Techniques:**
- 🚀 Server-side rendering (SSR) for initial load
- 📦 Code splitting by route
- 🎨 Image optimization (WebP, lazy loading)
- 💾 Aggressive caching strategy (CDN + Redis)
- 🔄 Database query optimization (indexed queries)
- 📡 WebSocket connection pooling
- 🧮 Memoization of expensive computations

---

## 🚀 Deployment Strategy

### Multi-Environment Pipeline

```mermaid
graph LR
    Dev[Development<br/>Local] --> Staging[Staging<br/>GCP Cloud Run]
    Staging --> Test[Automated Testing<br/>E2E + Integration]
    Test --> Approval{Manual Approval}
    Approval -->|Approved| Prod[Production<br/>Multi-region]
    Approval -->|Rejected| Dev
    Prod --> Monitor[Monitoring<br/>Datadog/Sentry]
    Monitor --> Alert{Issues?}
    Alert -->|Yes| Rollback[Automated Rollback]
    Alert -->|No| Success[Deployment Success]
    Rollback --> Dev

    style Dev fill:#61DAFB,stroke:#333,color:#000
    style Prod fill:#6BCF7F,stroke:#333,color:#000
    style Rollback fill:#FF6B6B,stroke:#333,color:#fff
```

**Infrastructure:**
- **Cloud Provider**: Google Cloud Platform (primary), AWS (backup)
- **Container Orchestration**: Kubernetes (GKE)
- **CI/CD**: GitHub Actions + GitLab CI
- **Monitoring**: Datadog, Sentry, Cloud Monitoring
- **Logging**: ELK Stack (Elasticsearch, Logstash, Kibana)

---

## 🛠️ Tech Stack

### Frontend Architecture
```typescript
// Core Stack
├── React 18.x (Concurrent Mode, Suspense)
├── TypeScript 5.x (Strict mode)
├── Vite (Build tool, HMR)
├── Tailwind CSS + shadcn/ui
├── React Query (Server state)
├── Zustand (Client state)
├── React Router v6 (Routing)
└── PWA (Service Workers, offline-first)
```

### Backend Architecture
```typescript
// API Layer
├── Node.js 20.x (LTS)
├── Express.js (REST API)
├── TypeScript (Type safety)
├── Zod (Runtime validation)
└── Pydantic (Python services)

// AI/ML Layer
├── Google Gemini API (Live + Flash)
├── LangChain (Orchestration)
├── LangGraph (Agent workflows)
├── OpenAI SDK (Fallback)
└── Custom tool executors
```

### Data Layer
```typescript
// Databases
├── PostgreSQL 15+ (Primary)
│   ├── Orders, Users, Menu
│   └── Prisma ORM
├── Redis 7+ (Cache + Sessions)
├── MongoDB (Analytics, Logs)
└── Cloud Storage (Media assets)
```

### DevOps & Infrastructure
```typescript
// Cloud & Deployment
├── GCP (Primary)
│   ├── Cloud Run (Containers)
│   ├── Cloud Functions (Serverless)
│   ├── Cloud SQL (Managed PostgreSQL)
│   └── Vertex AI (ML serving)
├── Docker + Kubernetes
├── GitHub Actions (CI/CD)
├── Terraform (IaC)
└── Datadog (Monitoring)
```

---

## ⚠️ Demo Limitations

> **Important**: This is a **portfolio demonstration repository** containing:

✅ **Included in Demo:**
- Complete documentation and architecture diagrams
- Configuration files and environment templates
- API contracts and type definitions
- Database schemas and migration patterns
- Simplified UI component examples

❌ **Not Included (Proprietary):**
- Full production source code
- AI agent implementation details
- Payment gateway integrations
- WhatsApp Business API integration
- Advanced optimization algorithms
- Production environment configurations
- Client-specific customizations

### 📞 **Interested in the Full Implementation?**

**Available to qualified clients and employers:**
- Complete source code walkthrough
- Live deployment demonstration
- Technical architecture review
- Custom feature development
- Production deployment assistance

**Contact**: [w3jdev@gmail.com](mailto:w3jdev@gmail.com) | [+60174106981](tel:+60174106981)

---

## 📖 Documentation

Explore detailed documentation:

- [📐 Architecture Overview](docs/ARCHITECTURE.md)
- [🎯 Product Specifications](docs/PRODUCT.md)
- [🗺️ Project Roadmap](docs/ROADMAP.md)
- [🤖 AI Architecture Analysis](docs/AI_ARCHITECTURE_ANALYSIS.md)
- [⚙️ Setup Guide](docs/setup/SETUP.md)
- [🎤 Pitch Deck](docs/PITCH_DECK.md)

---

## 💼 About the Creator

<div align="center">

### Muhammad Nurunnabi (W3JDEV)
**Senior Full-Stack AI Engineer | AI Agents & Automation Specialist**

📍 Kuala Lumpur, Malaysia

</div>

### 🎖️ Professional Highlights

- 🤖 **15+ Production AI Applications** with measurable business impact
- 💰 **300%+ ROI** delivered through AI automation solutions
- ⚡ **95% Task Reduction** in manual operational workflows
- 🏆 **GitHired Score: 93/100** | 1,799+ GitHub Contributions (2025)
- 🔒 **Zero Security Incidents** across all enterprise deployments
- 🚀 **Fortune 500 Experience**: CMA CGM (shipping & logistics)

### 🛠️ Core Expertise

**AI & Machine Learning:**
- LLM Orchestration (GPT-4, Gemini, Claude)
- Multi-agent Systems (LangChain, LangGraph, CrewAI)
- RAG Pipelines (FAISS, Pinecone, ChromaDB)
- Prompt Engineering & Fine-tuning
- Real-time AI Inference Optimization

**Full-Stack Development:**
- Frontend: React, TypeScript, Next.js
- Backend: Node.js, Python, FastAPI, Express
- Databases: PostgreSQL, MongoDB, Redis
- Cloud: GCP, Azure, AWS
- DevOps: Docker, Kubernetes, CI/CD

**System Design & Architecture:**
- Microservices Architecture
- Event-Driven Systems
- Real-time Data Pipelines
- API Design & Integration
- Performance Optimization

---

## 🌟 Featured Projects

### 🎙️ FlairAI - Multilingual Voice Coach
Real-time AI voice training system for hospitality staff
- **Tech**: Python, Node.js, React, Gemini, ElevenLabs
- **Impact**: 40% reduction in onboarding time, $24K annual savings
- **Status**: Live in 3+ F&B locations

### 👔 PunchClock - AI HR Automation
Zero-capex HR operating system for Malaysian SMEs
- **Tech**: Google Apps Script, Python, GPT-4, Cloud Vision
- **Impact**: 95% reduction in payroll processing time
- **Status**: Powering 3+ businesses

### 🍷 VineAI - Wine Recommendation Engine
Intelligent wine pairing via RAG pipeline
- **Tech**: Python, FAISS, OpenAI, spaCy
- **Impact**: 35% increase in ticket size, 300% ROI
- **Status**: Deployed to 50+ restaurants (SEA)

---

## 📬 Contact & Connect

<div align="center">

| Channel | Link |
|---------|------|
| 📧 **Email** | [w3jdev@gmail.com](mailto:w3jdev@gmail.com) |
| 📞 **Phone** | [+60174106981](tel:+60174106981) |
| 💼 **LinkedIn** | [linkedin.com/in/w3jdev](https://linkedin.com/in/w3jdev) |
| 🐦 **Twitter** | [@mnjewelps](https://twitter.com/mnjewelps) |
| 🔗 **GitHub** | [@W3JDev](https://github.com/W3JDev) |
| 🌐 **Portfolio** | [portfolio.w3jdev.com](https://portfolio.w3jdev.com) |

---

### 💼 Open to Opportunities

- ✅ Full-time Senior AI Engineer positions
- ✅ Contract/Consulting projects
- ✅ AI system architecture consulting
- ✅ Technical advisory roles

**Location**: Kuala Lumpur, Malaysia (Open to remote work)

</div>

---

## 📄 License

**© 2025 W3J LLC**  
This demonstration repository is provided for portfolio showcase purposes.  
Full production implementation is proprietary and available under commercial license.

---

## 🙏 Acknowledgments

Built with cutting-edge technology:
- [Google Gemini AI](https://ai.google.dev/) - Multimodal AI platform
- [React](https://react.dev/) - UI framework
- [shadcn/ui](https://ui.shadcn.com/) - Component library
- [Tailwind CSS](https://tailwindcss.com/) - Styling framework
- [LangChain](https://langchain.com/) - AI orchestration

---

<div align="center">

**⭐ If you find this project interesting, please star the repository!**

**For business inquiries or collaboration opportunities:**  
📧 [w3jdev@gmail.com](mailto:w3jdev@gmail.com) | 📞 [+60174106981](tel:+60174106981)

---

*"Building AI systems that solve real business problems, not just technical exercises."*  
**— Muhammad Nurunnabi**

</div>
