# GEO-Metrics

A microservices-based geometrics analysis platform built with modern web technologies and deployed on Kubernetes.

## Architecture

This project consists of several microservices:

- **Frontend**: Next.js 16 + React 19 + TypeScript + Tailwind CSS v4
- **LLM Service**: FastAPI Python service integrating HuggingFace models for AI-powered geometrics analysis
- **Report Service**: FastAPI Python service with database integration for report generation and storage
- **Authentication**: Ory stack (Kratos/Keto/Oathkeeper) for identity management and access control
- **Infrastructure**: Kubernetes with MetalLB load balancing, cert-manager, and ingress

## Project Structure

```
GEO-Metrics/
├── frontend/           # Next.js frontend application
├── llm-service/        # Python FastAPI service for LLM integration
├── report-service/     # Python FastAPI service for reports
├── k8s/               # Kubernetes deployment manifests
├── scripts/
│   └── setup/         # Setup scripts for deployment
└── README.md
```

## Prerequisites

- Docker and Docker Compose
- Kubernetes cluster (local or cloud)
- kubectl configured
- Helm 3.x
- Node.js 20+ (for frontend development)
- Python 3.9+ (for service development)

## Quick Start

### Development Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd GEO-Metrics
   ```

2. **Setup Infrastructure**
   ```bash
   cd ../infra
   ./scripts/setup/setup-infra.sh
   ```

3. **Setup Authentication (Ory)**
   ```bash
   cd ../ory
   ./scripts/setup/setup-ory.sh
   ```

4. **Setup GEO-Metrics Services**
   ```bash
   ./scripts/setup/setup-geometrics.sh
   ```

### Frontend Development

```bash
cd frontend
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to view the application.

### Service Development

For LLM Service:
```bash
cd llm-service
pip install -r requirements.txt
uvicorn main:app --reload
```

For Report Service:
```bash
cd report-service
pip install -r requirements.txt
uvicorn main:app --reload
```

## Deployment

### Automated Deployment

The project uses GitHub Actions for CI/CD. Images are automatically built and pushed to GitHub Container Registry (`ghcr.io`).

### Manual Deployment

1. **Build and push images** (handled by CI/CD)
2. **Deploy to Kubernetes**
   ```bash
   ./scripts/setup/setup-geometrics.sh
   ```

## Services

### Frontend Service
- **Port**: 3000
- **Image**: `ghcr.io/geo-metrics-project/frontend:latest`
- **Environment**: `NEXT_PUBLIC_API_URL=http://api-gateway:3000`

### LLM Service
- **Port**: 8081
- **Image**: `ghcr.io/geo-metrics-project/llm-service:latest`
- **Dependencies**: HuggingFace API (`HUGGINGFACE_API_KEY`)

### Report Service
- **Port**: 8080
- **Image**: `ghcr.io/geo-metrics-project/report-service:latest`
- **Dependencies**: PostgreSQL database

## API Documentation

Each service provides OpenAPI documentation:

- Frontend: N/A (client-side)
- LLM Service: `http://llm-service:8081/docs`
- Report Service: `http://report-service:8080/docs`

## Configuration

### Environment Variables

#### Frontend
- `PORT`: Server port (default: 3000)
- `NEXT_PUBLIC_API_URL`: API gateway URL

#### LLM Service
- `HUGGINGFACE_API_KEY`: HuggingFace API token
- `PORT`: Server port (default: 8081)

#### Report Service
- `DATABASE_URL`: PostgreSQL connection string
- `PORT`: Server port (default: 8080)

## Development Workflow

1. **Local Development**: Use `npm run dev` / `uvicorn --reload` for hot reloading
2. **Testing**: Run tests in each service directory
3. **Building**: Docker images are built via GitHub Actions
4. **Deployment**: Apply Kubernetes manifests using setup scripts

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

[Add license information here]
