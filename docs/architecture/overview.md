# HireSight Architecture Overview

## System Architecture

HireSight is built as a microservices architecture with the following components:

### Frontend (Next.js 15)
- **Technology**: Next.js 15 + React 19 + TypeScript
- **Purpose**: User interface and real-time interview assistant
- **Key Features**:
  - Dashboard for candidate management
  - Live interview interface with WebRTC
  - Real-time transcription and analysis
  - Semantic search interface

### Backend API (Node.js)
- **Technology**: Node.js + Express + TypeScript + Prisma
- **Purpose**: Core business logic and API endpoints
- **Key Features**:
  - RESTful API endpoints
  - Authentication and authorization
  - File processing orchestration
  - Real-time WebSocket connections

### Database (TiDB Cloud)
- **Technology**: TiDB Cloud (MySQL-compatible)
- **Purpose**: Primary data storage and vector embeddings
- **Key Features**:
  - Relational data storage
  - JSON columns for flexible metadata
  - Vector embeddings storage
  - High availability and scalability

### Queue System (BullMQ + Redis)
- **Technology**: BullMQ + Redis
- **Purpose**: Background job processing
- **Key Features**:
  - File processing jobs
  - AI analysis jobs
  - Email notifications
  - Retry and error handling

### Ingest Worker (Node.js)
- **Technology**: Node.js + TypeScript
- **Purpose**: File processing and data extraction
- **Key Features**:
  - CV parsing (PDF, DOC, DOCX)
  - LinkedIn profile scraping
  - GitHub repository analysis
  - Embedding generation

### ANN Worker (Python)
- **Technology**: Python + FastAPI + HNSWlib
- **Purpose**: Vector search and similarity matching
- **Key Features**:
  - Approximate nearest neighbor search
  - Index management and updates
  - Similarity scoring
  - Index persistence

### Storage (Azure Blob Storage)
- **Technology**: Azure Blob Storage
- **Purpose**: File storage and index snapshots
- **Key Features**:
  - CV file storage
  - Audio recording storage
  - Index snapshot storage
  - CDN integration

## Data Flow

### 1. Candidate Upload Flow
```
User Uploads CV → Backend API → Queue Job → Ingest Worker → 
File Processing → Embedding Generation → Database Storage → 
Index Update → ANN Worker
```

### 2. Live Interview Flow
```
WebRTC Audio → Backend API → ElevenLabs STT → 
Real-time Analysis → LLM Processing → 
Follow-up Suggestions → Frontend Display
```

### 3. Semantic Search Flow
```
Search Query → Frontend → Backend API → 
ANN Worker → Vector Search → 
Similarity Results → Database Query → 
Formatted Results → Frontend Display
```

## Technology Decisions

### Why TiDB Cloud?
- **MySQL Compatibility**: Easy migration from existing systems
- **Scalability**: Serverless architecture with auto-scaling
- **Performance**: Optimized for both OLTP and OLAP workloads
- **JSON Support**: Native support for vector embeddings
- **Global Distribution**: Multi-region deployment support

### Why Render?
- **Simplicity**: Easy deployment and management
- **Auto-scaling**: Automatic scaling based on demand
- **GitHub Integration**: Direct deployment from repository
- **Environment Management**: Easy configuration management
- **Cost-effective**: Pay-per-use pricing model

### Why BullMQ + Redis?
- **Reliability**: Persistent job queues with retry logic
- **Scalability**: Horizontal scaling of workers
- **Monitoring**: Built-in job monitoring and metrics
- **TypeScript Support**: Full TypeScript integration
- **Performance**: High-throughput job processing

## Security Considerations

### Data Protection
- **Encryption**: All data encrypted in transit and at rest
- **Access Control**: Role-based access control (RBAC)
- **Audit Logging**: Complete audit trail for all operations
- **Data Retention**: Automated data cleanup policies

### API Security
- **Rate Limiting**: Prevent abuse and DoS attacks
- **Input Validation**: Comprehensive input sanitization
- **CORS Configuration**: Proper cross-origin resource sharing
- **JWT Authentication**: Secure token-based authentication

### Privacy Compliance
- **Consent Management**: Candidate data consent flows
- **Right to Deletion**: GDPR-compliant data deletion
- **Data Minimization**: Only collect necessary data
- **Transparency**: Clear data usage policies

## Monitoring and Observability

### Logging
- **Structured Logging**: JSON-formatted logs with correlation IDs
- **Log Aggregation**: Centralized log collection and analysis
- **Log Levels**: Appropriate log levels for different environments
- **Performance Logging**: Request/response timing and metrics

### Metrics
- **Application Metrics**: Custom business metrics
- **Infrastructure Metrics**: System resource utilization
- **Error Rates**: API error rates and types
- **Performance Metrics**: Response times and throughput

### Alerting
- **Error Alerts**: Immediate notification of critical errors
- **Performance Alerts**: Response time threshold violations
- **Resource Alerts**: High resource utilization warnings
- **Business Alerts**: Important business metric changes

## Scalability Considerations

### Horizontal Scaling
- **Stateless Services**: All services designed to be stateless
- **Load Balancing**: Multiple instances behind load balancers
- **Database Sharding**: Potential for database sharding if needed
- **Worker Scaling**: Dynamic worker scaling based on queue depth

### Performance Optimization
- **Caching**: Redis caching for frequently accessed data
- **CDN**: Azure CDN for static file delivery
- **Database Indexing**: Optimized database indexes
- **Vector Indexing**: Efficient vector search with HNSW

### Cost Optimization
- **Serverless Architecture**: Pay-per-use pricing model
- **Auto-scaling**: Automatic scaling based on demand
- **Resource Optimization**: Right-sized instances
- **Data Lifecycle**: Automated data archival and cleanup
