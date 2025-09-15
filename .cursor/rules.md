# HireSight Project Rules & Guidelines

## Project Overview

HireSight is an AI-powered hiring verification and live interview assistant for hiring teams at startups and remote-first companies. It verifies candidate authenticity across CVs, LinkedIn and GitHub; produces a credibility score with explainable flags; provides live, real-time interview support (transcription, inconsistency detection, and suggested follow-ups); and stores all canonical metadata and vectors in TiDB Cloud for reliable, auditable retrieval.

## Architecture Guidelines

### High-Level Architecture
- **Frontend**: Next.js 15 + React 19 + TypeScript
- **Backend**: Node.js + TypeScript + Prisma (MySQL provider)
- **Database**: TiDB Cloud (MySQL-compatible)
- **Queue System**: BullMQ + Redis
- **Storage**: Azure Blob Storage
- **ANN Worker**: Python + FastAPI + HNSWlib
- **Deployment**: Render
- **LLM**: BlackBox AI o4-mini-high (primary), Moonshot AI Kimi K2 (fallback)

### Core Capabilities
1. **Multi-Source Profile Analysis**: CV parsing, LinkedIn/GitHub cross-referencing
2. **Credibility Scoring**: AI-powered 0-100 scoring with explainable flags
3. **Live Interview Assistant**: Real-time transcription and analysis
4. **Semantic Search**: Vector-based candidate retrieval
5. **Comprehensive Dashboard**: Unified candidate view and reporting

## Development Standards

### Code Style
- **Language**: TypeScript for all JavaScript/Node.js code
- **Formatting**: Prettier with 2-space indentation
- **Linting**: ESLint with TypeScript rules
- **Naming**: camelCase for variables/functions, PascalCase for components/classes
- **File Structure**: Feature-based organization within service directories

### TypeScript Guidelines
- Always use strict mode
- Define interfaces for all data structures
- Use proper typing for API responses and database models
- Avoid `any` type - use proper type definitions
- Use generics for reusable components

### API Design
- RESTful endpoints with clear naming conventions
- Consistent response format with status codes
- Proper error handling and validation
- API versioning in URL path (/api/v1/)
- OpenAPI/Swagger documentation

## Technology Stack Specifications

### Frontend Stack
```json
{
  "next": "^15.0.0",
  "react": "^19.0.0",
  "typescript": "^5.0.0",
  "tailwindcss": "^3.4.0",
  "@radix-ui/react-*": "^1.0.0",
  "framer-motion": "^10.0.0",
  "prisma": "^5.0.0",
  "@prisma/client": "^5.0.0"
}
```

### Backend Stack
```json
{
  "express": "^4.18.0",
  "typescript": "^5.0.0",
  "prisma": "^5.0.0",
  "mysql2": "^3.6.0",
  "bullmq": "^4.15.0",
  "redis": "^4.6.0",
  "openai": "^4.0.0",
  "axios": "^1.6.0"
}
```

### Python Worker Stack
```python
{
  "fastapi": "^0.104.0",
  "hnswlib": "^0.7.0",
  "numpy": "^1.24.0",
  "openai": "^1.0.0",
  "azure-storage-blob": "^12.19.0"
}
```

## Database Schema Requirements

### Core Tables
- **users**: User authentication and profiles
- **applicants**: Candidate information and metadata
- **files**: File storage references and metadata
- **embeddings**: Vector embeddings for semantic search
- **interviews**: Interview sessions and transcripts
- **flags**: Credibility flags and explanations
- **jobs**: Job postings and requirements

### Vector Storage
- Store embeddings as JSON arrays in TiDB Cloud
- Use HNSWlib for approximate nearest neighbor search
- Maintain index snapshots in Azure Blob Storage
- Support per-source embeddings (CV, LinkedIn, GitHub)

### Data Types
- Use MySQL-compatible data types
- JSON columns for flexible metadata storage
- Proper indexing for performance
- Foreign key relationships with proper constraints

## API Endpoint Documentation Standards

### Response Format
```typescript
interface APIResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
  message?: string;
  timestamp: string;
}
```

### Error Handling
- Use appropriate HTTP status codes
- Provide detailed error messages for debugging
- Log errors with proper context
- Return user-friendly error messages

### Authentication
- JWT-based authentication
- Role-based access control (RBAC)
- Secure token storage and refresh
- API key management for external services

## Security and Compliance Guidelines

### Data Protection
- Encrypt PII in transit and at rest
- Implement proper access controls
- Regular security audits and updates
- Secure API endpoints with rate limiting

### Privacy Compliance
- Candidate consent flow for data collection
- Data retention policies
- Right to deletion implementation
- Audit trails for data access

### External Integrations
- Secure API key management
- Rate limiting for external APIs
- Proper error handling for service failures
- Fallback mechanisms for critical services

## File Organization

### Directory Structure
```
hiresight-devpost/
├── .cursor/
│   └── rules.md
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   ├── components/
│   │   ├── lib/
│   │   └── types/
│   ├── package.json
│   └── next.config.js
├── backend/
│   ├── src/
│   │   ├── routes/
│   │   ├── services/
│   │   ├── middleware/
│   │   └── utils/
│   ├── package.json
│   └── server.js
├── workers/
│   ├── ingest/
│   │   ├── src/
│   │   ├── package.json
│   │   └── worker.js
│   └── ann/
│       ├── src/
│       ├── requirements.txt
│       └── main.py
├── infra/
│   ├── docker/
│   ├── render/
│   └── scripts/
├── docs/
│   ├── api/
│   ├── deployment/
│   └── architecture/
├── demo_sample_data/
└── README.md
```

## Development Workflow

### Git Workflow
- Use feature branches for development
- Commit messages follow conventional commits
- Pull requests require code review
- Main branch is always deployable

### Testing
- Unit tests for all business logic
- Integration tests for API endpoints
- E2E tests for critical user flows
- Performance testing for vector operations

### Deployment
- Automated deployment via GitHub Actions
- Environment-specific configurations
- Database migrations with rollback support
- Health checks and monitoring

## References

- [TiDB Cloud Documentation](https://docs.pingcap.com/tidbcloud/dev-guide-overview/)
- [Next.js 15 Documentation](https://nextjs.org/docs)
- [Prisma Documentation](https://www.prisma.io/docs)
- [BullMQ Documentation](https://docs.bullmq.io/)
- [HNSWlib Documentation](https://github.com/nmslib/hnswlib)
- [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/)
- [Render Documentation](https://render.com/docs)
