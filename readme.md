# End-to-End DevOps CI/CD Project with Jenkins, Docker & AWS EKS

## Project Overview

This project demonstrates an automated CI/CD pipeline that builds, stores, and deploys a web application using GitHub, Jenkins, Docker, Amazon ECR, and Amazon EKS.

When code is pushed to GitHub, Jenkins builds a Docker image, pushes it to Amazon ECR, and updates the application running on Kubernetes. The pipeline also retains deployment to an existing EC2 Docker server.

## Architecture

```text
Developer → GitHub → Webhook → Jenkins
                                |
                         Build Docker Image
                                |
                                v
                           Amazon ECR
                                |
                                v
                            Amazon EKS
                                |
                         Kubernetes Pod
                                |
                         NodePort Service
                                |
                            Web Browser
```

## Technologies Used

- **Git & GitHub:** Source code management
- **Jenkins:** CI/CD automation
- **Docker:** Application containerization
- **NGINX:** Web server
- **Amazon EC2:** Hosts Jenkins and the existing Docker application
- **Amazon ECR:** Stores Docker images
- **Amazon EKS:** Managed Kubernetes cluster
- **Kubernetes:** Deploys and manages application containers
- **AWS IAM & Security Groups:** Access control and networking

## Project Structure

```text
DevOps-CI-CD-Project/
├── index.html
├── Dockerfile
├── Jenkinsfile
├── README.md
└── k8s/
    ├── deployment.yaml
    └── service.yaml
```

- `index.html` — Website content.
- `Dockerfile` — Defines the NGINX-based Docker image.
- `Jenkinsfile` — Defines the automated build and deployment stages.
- `k8s/deployment.yaml` — Defines the Kubernetes application Deployment.
- `k8s/service.yaml` — Exposes the application through a NodePort Service.
- `README.md` — Project documentation.

## CI/CD Workflow

1. Push application changes to the GitHub `main` branch.
2. GitHub sends a webhook to Jenkins.
3. Jenkins checks out the latest source code.
4. Jenkins builds a Docker image with a unique build-number tag.
5. Jenkins authenticates with Amazon ECR and pushes the image.
6. Jenkins updates the Kubernetes Deployment on Amazon EKS.
7. Kubernetes rolls out the new application version.
8. The pipeline also deploys to the existing EC2 Docker server.

## Kubernetes Deployment

The application runs in a Kubernetes Pod managed by a Deployment.

- **EKS cluster:** `devops-eks-cluster`
- **AWS region:** `ap-south-1` (Mumbai)
- **Deployment:** `devops-web`
- **Service:** `devops-web-service`
- **Service type:** NodePort
- **NodePort:** `30080`
- **Container port:** `80`

The NodePort Service routes incoming traffic to the application Pods. During testing, the website was accessible through the EKS nodes on port `30080`.

## Useful Commands

Check cluster nodes:

```bash
kubectl get nodes
```

Check the Deployment and Pods:

```bash
kubectl get deployment devops-web
kubectl get pods -l app=devops-web
```

Check the Service:

```bash
kubectl get service devops-web-service
```

Verify deployment rollout:

```bash
kubectl rollout status deployment/devops-web
```

## Security

- Use IAM roles instead of hardcoded AWS access keys.
- Restrict Jenkins and application access through security groups.
- Use least-privilege IAM and Kubernetes permissions.
- Avoid exposing administration interfaces publicly.

## Key Learnings

- GitHub webhook integration with Jenkins
- CI/CD pipeline automation
- Docker image building and publishing
- Amazon ECR and Amazon EKS integration
- Kubernetes Deployments and Services
- AWS IAM permissions and security groups
- Deployment verification and troubleshooting

##  Project Status

Implemented and tested the CI/CD workflow, Docker image publishing, EKS deployment, and Kubernetes rollout verification.

## Author

**Aqsa Saudagar**

- GitHub: [Aqsa-Saudagar](https://github.com/Aqsa-Saudagar)
- Repository: [DevOps-CI-CD-Project](https://github.com/Aqsa-Saudagar/DevOps-CI-CD-Project)
