# System Architecture

## Overview
This project demonstrates an end-to-end CI/CD pipeline for a containerized web service deployed to Azure. The goal is to show how code moves from local development through automated validation, container image creation, registry publishing, and cloud deployment.

The application is a lightweight FastAPI service packaged with Docker. GitHub Actions is used to run CI checks and handle deployment. Azure Container Registry (ACR) stores built images, and Azure App Service hosts the deployed container.

## Architecture Goals
- demonstrate CI/CD pipeline design
- validate code automatically on push and pull request
- build and package the application as a Docker image
- publish the image to Azure Container Registry
- deploy the containerized service to Azure App Service
- verify deployment with a health endpoint

## Components

### 1. Application Service
A small FastAPI service provides the deployable application.

Responsibilities:
- expose REST endpoints such as `/health` and `/version`
- serve as the artifact being tested, containerized, and deployed
- provide a simple target for CI/CD validation and smoke testing

### 2. Source Control Repository
GitHub stores the full project source.

Contents:
- application code
- automated tests
- Dockerfile
- Docker Compose configuration
- GitHub Actions workflow files
- architecture and setup documentation

### 3. Continuous Integration Pipeline
The CI workflow runs on push and pull request.

Responsibilities:
- install dependencies
- run linting and formatting checks
- run automated tests
- confirm the application builds successfully

### 4. Container Build Process
Docker is used to package the application into a portable container image.

Responsibilities:
- define a consistent runtime environment
- build the deployable image
- ensure the same artifact can be used locally and in Azure

### 5. Azure Container Registry (ACR)
ACR stores versioned Docker images produced by the pipeline.

Responsibilities:
- hold built application images
- act as the image source for deployment
- separate image storage from application hosting

### 6. Continuous Deployment Pipeline
The CD workflow runs on merge to `main` or by manual trigger.

Responsibilities:
- authenticate with Azure
- build or retrieve the latest image
- push the image to ACR
- deploy the image to Azure App Service
- verify the deployed service is reachable

### 7. Azure App Service
Azure App Service hosts the containerized FastAPI application.

Responsibilities:
- run the deployed container image
- provide a public endpoint for testing
- serve as the cloud deployment target for the demo

## Technology Stack

### Application
- Python 3.12
- FastAPI
- Uvicorn

### Testing and Quality
- pytest
- ruff
- black

### Containerization
- Docker
- Docker Compose

### CI/CD
- GitHub Actions

### Azure Services
- Azure Container Registry (ACR)
- Azure App Service

## Local Development Flow
1. Developer pulls the repository locally.
2. Developer installs dependencies or runs the app with Docker.
3. Developer runs the application locally.
4. Developer runs linting and tests locally.
5. Developer commits and pushes changes to GitHub.

## CI Flow
1. A push or pull request triggers GitHub Actions.
2. The workflow checks out the repository.
3. Python dependencies are installed.
4. Linting and formatting checks run.
5. Automated tests run.
6. The workflow confirms the application is buildable.

## CD Flow
1. A merge to `main` or manual trigger starts the deployment workflow.
2. GitHub Actions authenticates to Azure.
3. A Docker image is built for the application.
4. The image is pushed to Azure Container Registry.
5. Azure App Service is updated to use the new image.
6. The deployed application is verified through a health check endpoint.

## Request and Deployment Flow
The runtime flow for the deployed application is:

User Request  
→ Azure App Service  
→ FastAPI Application  
→ HTTP Response

The delivery flow is:

Developer  
→ GitHub Repository  
→ GitHub Actions CI  
→ Docker Image Build  
→ Azure Container Registry  
→ Azure App Service  
→ Health Check Verification

## Diagram

```mermaid
flowchart LR
    A[Developer] --> B[GitHub Repository]
    B --> C[GitHub Actions CI]
    C --> D[Lint and Tests]
    D --> E[Docker Image Build]
    E --> F[Azure Container Registry]
    F --> G[Azure App Service]
    G --> H[Health Check]