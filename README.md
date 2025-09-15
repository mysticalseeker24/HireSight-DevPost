# HireSight - AI-Powered Hiring Verification & Live Interview Assistant

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Next.js](https://img.shields.io/badge/Next.js-000000?logo=next.js&logoColor=white)](https://nextjs.org/)
[![TiDB Cloud](https://img.shields.io/badge/TiDB%20Cloud-000000?logo=tidb&logoColor=white)](https://cloud.tidbcloud.com/)

## 🎯 Project Overview

HireSight is an advanced AI-powered hiring verification and live interview assistant designed for hiring teams at startups and remote-first companies. It provides comprehensive candidate verification across multiple sources, real-time interview support, and intelligent analysis to streamline the hiring process.

### Key Features

- **🔍 Multi-Source Profile Analysis**: Cross-reference CVs, LinkedIn, and GitHub profiles
- **📊 Credibility Scoring**: AI-powered 0-100 scoring with explainable flags
- **🎤 Live Interview Assistant**: Real-time transcription and intelligent follow-up suggestions
- **🔎 Semantic Search**: Vector-based candidate retrieval and similarity matching
- **📈 Comprehensive Dashboard**: Unified candidate view with detailed analytics

## 🏗️ Architecture

### Technology Stack

- **Frontend**: Next.js 15 + React 19 + TypeScript + Tailwind CSS
- **Backend**: Node.js + TypeScript + Express + Prisma
- **Database**: TiDB Cloud (MySQL-compatible)
- **Queue System**: BullMQ + Redis
- **Storage**: Azure Blob Storage
- **ANN Worker**: Python + FastAPI + HNSWlib
- **Deployment**: Render
- **LLM**: BlackBox AI o4-mini-high (primary), Moonshot AI Kimi K2 (fallback)

### System Components

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Backend       │    │   Workers       │
│   (Next.js)     │◄──►│   (Node.js)     │◄──►│   (Node/Python) │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   TiDB Cloud    │    │   Azure Blob    │    │   Redis Queue   │
│   (Database)    │    │   (Storage)     │    │   (Jobs)        │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ and npm/yarn
- Python 3.9+
- TiDB Cloud account
- Azure Blob Storage account
- Redis instance

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/mysticalseeker24/HireSight-DevPost.git
   cd HireSight-DevPost
   ```

2. **Install dependencies**
   ```bash
   # Frontend
   cd frontend
   npm install
   
   # Backend
   cd ../backend
   npm install
   
   # Workers
   cd ../workers/ingest
   npm install
   
   cd ../ann
   pip install -r requirements.txt
   ```

3. **Environment Setup**
   ```bash
   # Copy environment templates
   cp .env.example .env.local
   
   # Configure your environment variables
   # See docs/deployment/environment-setup.md for details
   ```

4. **Database Setup**
   ```bash
   # Run Prisma migrations
   cd backend
   npx prisma migrate dev
   npx prisma generate
   ```

5. **Start Development Servers**
   ```bash
   # Terminal 1: Frontend
   cd frontend
   npm run dev
   
   # Terminal 2: Backend
   cd backend
   npm run dev
   
   # Terminal 3: Ingest Worker
   cd workers/ingest
   npm run dev
   
   # Terminal 4: ANN Worker
   cd workers/ann
   python main.py
   ```

## 📁 Project Structure

```
hiresight-devpost/
├── .cursor/                 # Cursor IDE rules and guidelines
├── frontend/               # Next.js frontend application
│   ├── src/
│   │   ├── app/           # Next.js app router
│   │   ├── components/    # Reusable UI components
│   │   ├── lib/          # Utility functions and services
│   │   └── types/        # TypeScript type definitions
│   └── package.json
├── backend/               # Node.js backend API
│   ├── src/
│   │   ├── routes/       # API route handlers
│   │   ├── services/     # Business logic services
│   │   ├── middleware/   # Express middleware
│   │   └── utils/        # Utility functions
│   └── package.json
├── workers/              # Background workers
│   ├── ingest/          # File processing worker
│   └── ann/             # Vector search worker
├── infra/               # Infrastructure and deployment
│   ├── docker/          # Docker configurations
│   ├── render/          # Render deployment configs
│   └── scripts/         # Deployment scripts
├── docs/                # Project documentation
│   ├── api/            # API documentation
│   ├── deployment/     # Deployment guides
│   └── architecture/   # Architecture documentation
├── demo_sample_data/    # Sample data for testing
└── README.md
```

## 🔧 Development

### Code Standards

- **TypeScript**: Strict mode enabled, proper typing required
- **ESLint**: Configured with TypeScript rules
- **Prettier**: Code formatting with 2-space indentation
- **Conventional Commits**: Standardized commit messages

### API Documentation

- **OpenAPI/Swagger**: Available at `/api/docs` in development
- **Postman Collection**: Available in `docs/api/`
- **TypeScript Types**: Auto-generated from Prisma schema

### Testing

```bash
# Run all tests
npm run test

# Run specific test suites
npm run test:unit
npm run test:integration
npm run test:e2e
```

## 🚀 Deployment

### Production Deployment

1. **TiDB Cloud Setup**
   - Create serverless cluster
   - Configure connection strings
   - Run production migrations

2. **Azure Blob Storage**
   - Create storage account
   - Configure containers
   - Set up access policies

3. **Render Deployment**
   - Configure services
   - Set environment variables
   - Deploy with GitHub Actions

See `docs/deployment/` for detailed deployment guides.

## 📊 Features

### Multi-Source Profile Analysis

- **CV Processing**: PDF/DOC/DOCX parsing with OCR fallback
- **LinkedIn Integration**: Profile scraping and verification
- **GitHub Analysis**: Repository quality and activity analysis
- **Cross-Reference Detection**: Identify inconsistencies across sources

### Credibility Scoring

- **AI-Powered Analysis**: LLM-based reasoning for scoring
- **Explainable Flags**: Clear explanations for each credibility issue
- **0-100 Scale**: Standardized scoring system
- **Historical Tracking**: Score evolution over time

### Live Interview Assistant

- **Real-Time Transcription**: WebRTC audio streaming
- **Inconsistency Detection**: AI-powered analysis during interviews
- **Follow-Up Suggestions**: Intelligent question recommendations
- **Transcript Analysis**: Real-time insights and highlights

### Semantic Search

- **Vector Embeddings**: Per-source embedding generation
- **ANN Search**: Fast approximate nearest neighbor search
- **Similarity Matching**: Find similar candidates and profiles
- **Contextual Retrieval**: Search based on skills, experience, and more

## 🔒 Security & Privacy

- **Data Encryption**: PII encrypted in transit and at rest
- **Access Controls**: Role-based permissions
- **Audit Trails**: Complete activity logging
- **Consent Management**: Candidate data consent flows
- **Retention Policies**: Automated data cleanup

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built for the RAISE YOUR HACK 2025 hackathon
- Inspired by the need for better hiring verification tools
- Powered by cutting-edge AI and vector search technologies

## 📞 Support

- **Documentation**: [docs/](docs/)
- **Issues**: [GitHub Issues](https://github.com/mysticalseeker24/HireSight-DevPost/issues)
- **Discussions**: [GitHub Discussions](https://github.com/mysticalseeker24/HireSight-DevPost/discussions)

---

**Built with ❤️ for better hiring decisions**
