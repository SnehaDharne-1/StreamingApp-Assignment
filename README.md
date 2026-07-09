# StreamingApp

Stream premium video content, host live watch parties, and manage your catalogue with a modern microservice architecture. The platform now ships with a production-ready admin portal, real-time chat, S3-backed adaptive streaming, and a redesigned cinematic frontend experience.

## Architecture

| Service | Port | Description |
| --- | --- | --- |
| `authService` | 3001 | User authentication, registration, JWT issuance |
| `streamingService` | 3002 | Video catalogue, S3 playback endpoints, public APIs |
| `adminService` | 3003 | Dedicated admin microservice for asset management and uploads |
| `chatService` | 3004 | Websocket + REST chat for live watch parties |
| `frontend` | 3000 | React SPA with revamped UI and integrated chat |
| `mongo` | 27017 | Shared MongoDB instance |

All backend services share common database models and utilities through `backend/common`.

## Environment Configuration

Create an `.env` for each service (or export variables before running). All services accept the standard AWS credentials for S3 access.

### Auth Service (`backend/authService/.env`)
```ini
PORT=3001
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=ap-south-1
AWS_S3_BUCKET=
```

### Streaming Service (`backend/streamingService/.env`)
```ini
PORT=3002
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=ap-south-1
AWS_S3_BUCKET=
AWS_CDN_URL=
STREAMING_PUBLIC_URL=http://localhost:3002
```

### Admin Service (`backend/adminService/.env`)
```ini
PORT=3003
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=ap-south-1
AWS_S3_BUCKET=
```

### Chat Service (`backend/chatService/.env`)
```ini
PORT=3004
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
```

### Frontend build variables (`frontend/.env` or Docker build args)
```ini
REACT_APP_AUTH_API_URL=http://localhost:3001/api
REACT_APP_STREAMING_API_URL=http://localhost:3002/api
REACT_APP_STREAMING_PUBLIC_URL=http://localhost:3002
REACT_APP_ADMIN_API_URL=http://localhost:3003/api/admin
REACT_APP_CHAT_API_URL=http://localhost:3004/api/chat
REACT_APP_CHAT_SOCKET_URL=http://localhost:3004
```

## Running with Docker Compose

1. Populate the environment variables above (or rely on the defaults baked into `docker-compose.yml`).
2. Build and start the stack:
   ```bash
   docker-compose up --build
   ```
3. Navigate to `http://localhost:3000` for the web app.

The compose file provisions MongoDB plus all four Node.js microservices. S3 credentials are optional for local testing—you can still browse seeded metadata, but streaming requires valid S3 objects.

## Local Development

Install dependencies for each service:

```bash
# auth service
cd backend/authService && npm install

# streaming service
cd ../streamingService && npm install

# admin service
cd ../adminService && npm install

# chat service
cd ../chatService && npm install

# frontend
cd ../../frontend && npm install
```

Run the services (in separate terminals) after starting MongoDB:

```bash
cd backend/authService && npm run dev
cd backend/streamingService && npm run dev
cd backend/adminService && npm run dev
cd backend/chatService && npm run dev
cd frontend && npm start
```

## Feature Highlights

- **S3-backed adaptive streaming** with secure signed uploads for admins.
- **Dedicated admin microservice** for video ingestion, metadata management, and featured curation.
- **Real-time chat** overlay in the player (Socket.IO + persistent message history).
- **Modern React experience** featuring cinematic hero sections, dynamic carousels, and responsive design.
- **Role-aware access control** across frontend routes and backend microservices.

## Testing

Automated tests are not yet included. Recommended smoke checks:

1. Register and log in through the web UI.
2. Upload a small video + thumbnail via the admin dashboard (requires valid S3 credentials).
3. Confirm playback from the browse page and verify that chat messages broadcast between multiple browser tabs.


# StreamingApp - Kubernetes Deployment on Amazon EKS

## Project Overview

This project demonstrates the deployment of a MERN-based Streaming Application using modern DevOps practices.

The application consists of multiple microservices that are containerized using Docker, stored in Amazon Elastic Container Registry (ECR), and deployed to an Amazon Elastic Kubernetes Service (EKS) cluster using Helm Charts.

---

# Project Architecture

```
GitHub Repository
        │
        ▼
Jenkins Pipeline
        │
        ▼
Docker Build
        │
        ▼
Amazon ECR
        │
        ▼
Amazon EKS Cluster
        │
        ▼
Helm Chart Deployment
        │
        ▼
Kubernetes Pods & Services
```

---

# Technology Stack

- Frontend : ReactJS
- Backend : NodeJS + Express
- Database : MongoDB
- Containerization : Docker
- CI/CD : Jenkins
- Container Registry : Amazon ECR
- Orchestration : Kubernetes
- Cluster : Amazon EKS
- Package Manager : Helm
- Cloud Platform : AWS

---

# Project Structure

```
StreamingApp-Assignment
│
├── frontend/
├── backend/
│   ├── authService/
│   ├── adminService/
│   ├── chatService/
│   └── streamingService/
│
├── streamingapp/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       ├── namespace.yaml
│       ├── mongo-deployment.yaml
│       ├── mongo-service.yaml
│       ├── frontend-deployment.yaml
│       ├── frontend-service.yaml
│       ├── auth-deployment.yaml
│       ├── auth-service.yaml
│       ├── admin-deployment.yaml
│       ├── admin-service.yaml
│       ├── streaming-deployment.yaml
│       ├── streaming-service.yaml
│       ├── chat-deployment.yaml
│       └── chat-service.yaml
│
├── Jenkinsfile
└── README.md
```

---

# Jenkins Pipeline Stages

The Jenkins pipeline automates the following tasks:

- Checkout Source Code
- Login to Amazon ECR
- Build Frontend Docker Image
- Build Auth Service Docker Image
- Build Streaming Service Docker Image
- Build Admin Service Docker Image
- Build Chat Service Docker Image
- Tag Docker Images
- Push Images to Amazon ECR

Pipeline Execution:

```
Checkout SCM
      ↓
Login to Amazon ECR
      ↓
Build Docker Images
      ↓
Tag Images
      ↓
Push Images to ECR
```

---

# Amazon ECR Repositories

The following repositories were created:

- streamingapp-frontend
- streamingapp-auth
- streamingapp-admin
- streamingapp-chat
- streamingapp-streaming

---

# Amazon EKS Cluster

Cluster Name

```
streamingapp-cluster
```

Node Group

```
streamingapp-nodes
```

Node Type

```
t3.medium
```

Number of Nodes

```
2
```

Region

```
ap-south-1
```

---

# Kubernetes Resources

The following resources are deployed:

### Deployments

- frontend
- auth
- admin
- streaming
- chat
- mongo

### Services

- frontend (LoadBalancer)
- auth (ClusterIP)
- admin (ClusterIP)
- streaming (ClusterIP)
- chat (ClusterIP)
- mongo (ClusterIP)

---

# MongoDB Configuration

MongoDB runs as an internal Kubernetes Deployment.

Applications connect using:

```
mongodb://mongo:27017/streamingapp
```

instead of

```
mongodb://localhost:27017/streamingapp
```

This change resolved the CrashLoopBackOff issue encountered during deployment.

---

# Helm Deployment

Create Helm Chart

```bash
helm create streamingapp
```

Install Chart

```bash
helm install streamingapp-release ./streamingapp
```

Upgrade Chart

```bash
helm upgrade streamingapp-release ./streamingapp
```

Check Releases

```bash
helm list
```

---

# Kubernetes Commands

View Nodes

```bash
kubectl get nodes
```

View Pods

```bash
kubectl get pods -n streamingapp
```

View Services

```bash
kubectl get svc -n streamingapp
```

View Deployments

```bash
kubectl get deployment -n streamingapp
```

View All Resources

```bash
kubectl get all -n streamingapp
```

View Logs

```bash
kubectl logs <pod-name> -n streamingapp
```

---

# Application Deployment Status

All application components are successfully deployed.

| Component | Status |
|-----------|--------|
| Frontend | Running |
| Auth Service | Running |
| Admin Service | Running |
| Streaming Service | Running |
| Chat Service | Running |
| MongoDB | Running |

---

# Frontend Access

The frontend is exposed using a Kubernetes LoadBalancer.

```
```

The DNS name can be obtained using:

```bash
kubectl get svc -n streamingapp
```

---

# Challenges Faced

### 1. IAM Permission Errors

Issue:

```
AccessDeniedException
```

Solution:

Attached AdministratorAccess policy to the IAM user.

---

### 2. CrashLoopBackOff

Issue:

```
MongoDB Connection Refused
```

Reason:

Applications were attempting to connect to

```
localhost:27017
```

inside Kubernetes Pods.

Solution:

Created MongoDB Deployment and Service.

Updated all services to use

```
mongodb://mongo:27017/streamingapp
```

---

### 3. Helm YAML Parsing Error

Issue:

```
yaml: did not find expected key
```

Reason:

Incorrect indentation in deployment YAML.

Solution:

Corrected YAML indentation and successfully upgraded the Helm release.

---

# Final Outcome

Successfully completed:

- Docker Containerization
- Jenkins CI Pipeline
- Amazon ECR Integration
- Amazon EKS Cluster Setup
- Kubernetes Deployments
- Kubernetes Services
- MongoDB Deployment
- Helm Chart Deployment
- LoadBalancer Configuration
- End-to-End Microservices Deployment

---

# Author

**Sneha Dharne**

## License

MIT © StreamFlix Team
