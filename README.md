# Project 2: Container Security

## Overview

Docker containerisation and deployment to Amazon ECS. Container scanning with Trivy and runtime security testing with OWASP ZAP. CI/CD pipeline automates build, scan, deploy, and test stages.

## Technologies

- Docker - Application containerisation
- Amazon ECS - Container orchestration
- Trivy - Container vulnerability scanning
- OWASP ZAP - Dynamic application security testing
- GitHub Actions - CI/CD automation
- Application Load Balancer - Static endpoint for ECS service

## Implementation

### CI/CD Workflow

#### Job 1: Build, Scan & Push
- Build Docker image
- Scan with Trivy (fails on HIGH/CRITICAL findings)
- Push to Docker Hub

#### Job 2: ECS Deployment
- AWS credentials from GitHub Secrets
- Force new deployment on ECS cluster
- Rolling update with new image

#### Job 3: Runtime DAST
- Wait for ECS stabilisation
- OWASP ZAP scans application via load balancer
- Generate vulnerability report

### Load Balancer Configuration

Application Load Balancer provides static endpoint for:
- Consistent user access
- OWASP ZAP scanning target
- Routes traffic to dynamic ECS task IPs

## Security Features

- Base image vulnerability scanning
- Container security scanning before deployment
- Runtime security testing post-deployment
- IAM roles with least privilege for ECS
- Rolling deployments maintain availability

## Project Structure

```
Project-2-Container-Security/
├── Dockerfile           # Container definition
├── .github/
│   └── workflows/
│       └── pipeline.yml # CI/CD configuration
├── terraform/
│   └── ecs.tf          # ECS infrastructure
└── app/                # Application code
```

## AWS Resources

- ECS cluster and service
- Task definitions
- Application Load Balancer
- Security groups
- IAM roles and policies