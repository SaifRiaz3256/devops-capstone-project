# DevOps Capstone Project – End-to-End CI/CD on Kubernetes

## Overview

This repository contains a complete **end-to-end DevOps practice project** demonstrating how to build, test, containerize, and deploy a Python microservice using **modern cloud-native DevOps tooling**.

The project showcases a **production-grade CI/CD pipeline** implemented with **Tekton**, deployment on **OpenShift (Kubernetes)**, secure containerization with **Docker**, and automated quality gates using **linting and unit tests**.

This repository is suitable for:

* DevOps engineers learning CI/CD
* Cloud engineers practicing Kubernetes & OpenShift
* Software engineers transitioning to DevOps
* Interview / portfolio demonstration

---

## Architecture at a Glance

```
GitHub Repo
   │
   ▼
Tekton Pipeline (CI/CD)
   ├── Clone Source
   ├── Lint (flake8)
   ├── Test (nosetests)
   ├── Build Image (buildah)
   └── Deploy to OpenShift
          │
          ▼
Kubernetes Deployment (3 replicas)
   │
   ▼
PostgreSQL (OpenShift Template)
```

---

## Technology Stack

| Layer            | Technology       |
| ---------------- | ---------------- |
| Language         | Python 3.9       |
| Framework        | Flask            |
| WSGI Server      | Gunicorn         |
| Database         | PostgreSQL       |
| Containerization | Docker           |
| CI/CD            | Tekton Pipelines |
| Container Build  | Buildah          |
| Orchestration    | Kubernetes       |
| Platform         | OpenShift        |
| Linting          | flake8           |
| Testing          | nosetests        |

---

## Project Structure

```
devops-capstone-project/
├── service/                 # Flask microservice
│   ├── __init__.py
│   ├── routes.py
│   ├── models.py
│   ├── config.py
│   └── common/
│       ├── error_handlers.py
│       ├── log_handlers.py
│       └── status.py
├── tests/                   # Unit tests
│   ├── test_models.py
│   └── test_routes.py
├── deploy/                  # Kubernetes manifests
│   ├── deployment.yaml
│   └── service.yaml
├── tekton/                  # CI/CD pipeline definitions
│   ├── pipeline.yaml
│   ├── tasks.yaml
│   └── pvc.yaml
├── Dockerfile
├── requirements.txt
└── README.md
```

---

## Application Features

* RESTful Flask API
* PostgreSQL persistence
* Structured error handling
* Environment-based configuration
* Production-ready WSGI deployment

---

## Docker Support

### Dockerfile Highlights

* Uses `python:3.9-slim`
* Installs dependencies via `requirements.txt`
* Runs as **non-root user**
* Uses **Gunicorn** as the entry point

### Build Image

```bash
docker build -t accounts .
```

### Run Container

```bash
docker run --rm -p 8080:8080 \
  -e DATABASE_URI=postgresql://user:pass@postgres:5432/db \
  accounts
```

---

## Kubernetes Deployment

### PostgreSQL

* Deployed using OpenShift `postgresql-ephemeral` template
* Credentials injected via Kubernetes Secrets

### Application Deployment

* 3 replicas for high availability
* Environment variables injected at runtime
* Service exposed internally and via OpenShift Route

### Verify Deployment

```bash
oc get all -l app=accounts
```

---

## CI/CD with Tekton

### Pipeline Stages

| Stage  | Purpose                             |
| ------ | ----------------------------------- |
| clone  | Clone GitHub repository             |
| lint   | Code linting with flake8            |
| tests  | Unit tests with nosetests           |
| build  | Build container image using buildah |
| deploy | Deploy to OpenShift                 |

### Run Pipeline

```bash
tkn pipeline start cd-pipeline \
  -p repo-url=https://github.com/<your-account>/devops-capstone-project.git \
  -p branch=main \
  -p build-image=image-registry.openshift-image-registry.svc:5000/<namespace>/accounts:latest \
  -w name=pipeline-workspace,claimName=pipelinerun-pvc \
  -s pipeline \
  --showlog
```

---

## Quality Gates

* **Linting enforced**: Pipeline fails on flake8 errors
* **Unit tests mandatory**: Pipeline stops if tests fail
* **Build only after validation**
* **Deploy only after successful build**

---

## Security Best Practices

* Non-root Docker containers
* Secrets managed via OpenShift
* No credentials stored in source code
* Least-privilege execution

---

## Git Workflow

* Feature branches for each milestone
* Pull Requests for all changes
* CI validation before merge
* Main branch always deployable

---

## Evidence & Verification

* Tekton PipelineRun logs
* Kubernetes resource validation
* Live service via OpenShift Route
* Automated, repeatable deployments

---

## Learning Outcomes

This project demonstrates:

* End-to-end CI/CD design
* Kubernetes production deployment
* Tekton pipeline authoring
* Container security best practices
* OpenShift operational workflows

---

## Future Enhancements

* GitHub Webhook triggers
* ArgoCD GitOps deployment
* Helm charts
* Prometheus & Grafana monitoring
* Blue/Green or Canary deployments

---

## License

This project is provided for **learning and demonstration purposes**.

---

## Author

**DevOps Capstone Practice Project**
Built to demonstrate real-world DevOps engineering skills.
