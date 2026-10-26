# 🎮 Tic-Tac-Toe Game | Docker + Amazon EKS

![AWS](https://img.shields.io/badge/AWS-EKS-orange?logo=amazon-aws)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue?logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Deployment-326CE5?logo=kubernetes)
![Docker Hub](https://img.shields.io/badge/Docker%20Hub-Image-2496ED?logo=docker)
![License](https://img.shields.io/badge/License-MIT-green)

A containerized **Tic-Tac-Toe web application** deployed on **Amazon Elastic Kubernetes Service (EKS)** using Docker, Kubernetes Deployment, and a LoadBalancer Service.

This project demonstrates a practical **Cloud & DevOps deployment workflow**, from building a Docker image to deploying and scaling the application on AWS EKS.

---

## 📌 Project Overview

The application is first packaged into a Docker container and pushed to Docker Hub. An Amazon EKS cluster is then created using `eksctl`, and Kubernetes is used to deploy, expose, monitor, and scale the application.

### Deployment Flow

```text
                 ┌─────────────────────┐
                 │   Tic-Tac-Toe App   │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │       Docker        │
                 │    Build Image      │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │     Docker Hub      │
                 │   Image Repository  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │     Amazon EKS      │
                 │  Kubernetes Cluster │
                 └──────────┬──────────┘
                            │
                     ┌──────┴──────┐
                     ▼             ▼
                  Pod 1          Pod 2
                     │             │
                     └──────┬──────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Kubernetes Service  │
                 │    LoadBalancer     │
                 └──────────┬──────────┘
                            │
                            ▼
                       🌐 Internet
                            │
                            ▼
                   🎮 Tic-Tac-Toe
```

---

# ✨ Features

* 🎮 Interactive Tic-Tac-Toe web application
* 🐳 Docker containerization
* 📦 Docker Hub image hosting
* ☁️ Amazon EKS deployment
* ☸️ Kubernetes Deployment
* 🌐 AWS LoadBalancer Service
* 📈 Kubernetes horizontal replica scaling
* 🔍 Pod and application monitoring
* 🛠️ Kubernetes troubleshooting commands
* 🔐 AWS IAM and CLI configuration
* 🧹 Complete deployment cleanup

---

# 🛠️ Technologies

| Technology       | Purpose                      |
| ---------------- | ---------------------------- |
| HTML             | Application UI               |
| CSS              | Application Styling          |
| JavaScript       | Game Logic                   |
| Docker           | Containerization             |
| Docker Hub       | Container Registry           |
| AWS IAM          | Identity & Access Management |
| AWS CLI          | AWS Management               |
| Amazon EKS       | Managed Kubernetes           |
| Kubernetes       | Container Orchestration      |
| eksctl           | EKS Cluster Creation         |
| kubectl          | Kubernetes CLI               |
| AWS LoadBalancer | Public Application Access    |

---

# 📂 Project Structure

```text
ujwals-tic-tac-toe-game/
│
├── index.html
├── Dockerfile
├── deployment.yaml
├── service.yaml
└── README.md
```

---

# ⚙️ Prerequisites

Make sure the following tools are installed:

* AWS CLI
* Docker
* kubectl
* eksctl
* AWS account
* Docker Hub account

Verify the installations:

```bash
aws --version
docker --version
kubectl version --client
eksctl version
```

---

# 🔐 1. Configure AWS

Create an IAM user with the permissions required for this project.

Configure AWS CLI:

```bash
aws configure
```

Enter your credentials:

```text
AWS Access Key ID: YOUR_ACCESS_KEY
AWS Secret Access Key: YOUR_SECRET_KEY
Default region name: ap-south-1
Default output format: json
```

Verify your AWS identity:

```bash
aws sts get-caller-identity
```

### 🔒 Security Recommendation

**Never commit AWS credentials to GitHub.**

Do not add:

```text
AWS Access Key
AWS Secret Access Key
.env
credentials
```

to your repository.

---

# 🐳 2. Build Docker Image

Clone the repository:

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

Go to the project directory:

```bash
cd ujwals-tic-tac-toe-game
```

Build the Docker image:

```bash
docker build -t ujwals-tic-tac-toe-game:v1 .
```

Check the image:

```bash
docker images
```

---

# ▶️ 3. Run Application Locally

Start the container:

```bash
docker run -d --name cont1 -p 80:80 ujwals-tic-tac-toe-game:v1
```

Check the container:

```bash
docker ps
```

Open the application:

```text
http://localhost
```

Stop the container:

```bash
docker stop cont1
```

Remove the container:

```bash
docker rm cont1
```

---

# 📦 4. Push Docker Image to Docker Hub

Login to Docker Hub:

```bash
docker login -u tinu10
```

Tag the image:

```bash
docker tag ujwals-tic-tac-toe-game:v1 tinu10/ujwals-tic-tac-toe-game:v1
```

Push the image:

```bash
docker push tinu10/ujwals-tic-tac-toe-game:v1
```

Docker image:

```text
tinu10/ujwals-tic-tac-toe-game:v1
```

---

# ☸️ 5. Create Amazon EKS Cluster

Create the EKS cluster:

```bash
eksctl create cluster \
  --name game-cluster \
  --region ap-south-1 \
  --nodegroup-name game-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3
```

### Cluster Configuration

```text
Cluster Name:     game-cluster
Region:           ap-south-1
Node Group:       game-nodes
Instance Type:    t3.medium
Desired Nodes:    2
Minimum Nodes:    1
Maximum Nodes:    3
```

Check the cluster:

```bash
eksctl get cluster
```

---

# 🔎 6. Verify Kubernetes Nodes

Run:

```bash
kubectl get nodes
```

Expected result:

```text
NAME                           STATUS   ROLES
ip-xxx-xxx-xxx-xxx            Ready    <none>
ip-xxx-xxx-xxx-xxx            Ready    <none>
```

All nodes should show:

```text
STATUS: Ready
```

---

# 🚀 7. Kubernetes Deployment

Create a file named:

```text
deployment.yaml
```

Apply the Deployment:

```bash
kubectl apply -f deployment.yaml
```

---

# 🔍 8. Verify Deployment

Check Deployments:

```bash
kubectl get deployments
```

Check Pods:

```bash
kubectl get pods
```

Example:

```text
NAME                           READY   STATUS    RESTARTS
tic-tac-toe-xxxxxxxxx-xxxxx    1/1     Running   0
tic-tac-toe-xxxxxxxxx-xxxxx    1/1     Running   0
```

---

# 🌐 9. Create LoadBalancer Service

Create:

```text
service.yaml
```
Apply the Service:

```bash
kubectl apply -f service.yaml
```

---

# 🌍 10. Access the Application

Check the Service:

```bash
kubectl get svc
```

Example:

```text
NAME                   TYPE           EXTERNAL-IP
tic-tac-toe-service    LoadBalancer   xxxxx.ap-south-1.elb.amazonaws.com
```

Copy the `EXTERNAL-IP` and open it in your browser:

```text
http://xxxxx.ap-south-1.elb.amazonaws.com
```

🎉 The Tic-Tac-Toe application is now publicly accessible.

---

# 📊 11. Kubernetes Monitoring

### Get Nodes

```bash
kubectl get nodes
```

### Get Pods

```bash
kubectl get pods
```

### Get Deployments

```bash
kubectl get deployments
```

### Get Services

```bash
kubectl get svc
```

### Detailed Pod Information

```bash
kubectl describe pod <pod-name>
```

### View Pod Logs

```bash
kubectl logs <pod-name>
```

---

# 📈 12. Scale the Application

Scale the application to 5 replicas:

```bash
kubectl scale deployment tic-tac-toe --replicas=5
```

Verify:

```bash
kubectl get pods
```

You should now have 5 application Pods.

Check Deployment:

```bash
kubectl get deployment tic-tac-toe
```

---

# 🔄 Application Update

When a new Docker image version is available:

```bash
docker build -t ujwals-tic-tac-toe-game:v2 .
```

Tag it:

```bash
docker tag ujwals-tic-tac-toe-game:v2 tinu10/ujwals-tic-tac-toe-game:v2
```

Push it:

```bash
docker push tinu10/ujwals-tic-tac-toe-game:v2
```

Update Kubernetes:

```bash
kubectl set image deployment/tic-tac-toe \
tic-tac-toe=tinu10/ujwals-tic-tac-toe-game:v2
```

Check rollout:

```bash
kubectl rollout status deployment/tic-tac-toe
```

---

# 🧪 Troubleshooting

### Check Pod Status

```bash
kubectl get pods
```

### Describe Pod

```bash
kubectl describe pod <pod-name>
```

### Check Logs

```bash
kubectl logs <pod-name>
```

### Check Service

```bash
kubectl get svc
```

### Check Deployment

```bash
kubectl describe deployment tic-tac-toe
```

### Check All Resources

```bash
kubectl get all
```

---

# 🧹 13. Cleanup

Delete the Kubernetes Deployment:

```bash
kubectl delete -f deployment.yaml
```

Delete the LoadBalancer Service:

```bash
kubectl delete -f service.yaml
```

Finally, delete the EKS cluster:

```bash
eksctl delete cluster \
  --name game-cluster \
  --region ap-south-1
```

> ⚠️ Delete the EKS cluster after testing to avoid unnecessary AWS charges.

---

# 📋 Complete Command Reference

```bash
# Docker
docker build -t ujwals-tic-tac-toe-game:v1 .
docker images
docker run -d --name cont1 -p 80:80 ujwals-tic-tac-toe-game:v1
docker ps

# Docker Hub
docker login -u tinu10
docker tag ujwals-tic-tac-toe-game:v1 tinu10/ujwals-tic-tac-toe-game:v1
docker push tinu10/ujwals-tic-tac-toe-game:v1

# AWS
aws --version
aws configure
aws sts get-caller-identity

# EKS
eksctl version
eksctl create cluster --name game-cluster --region ap-south-1 --nodegroup-name game-nodes --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3

# Kubernetes
kubectl version --client
kubectl get nodes
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods
kubectl apply -f service.yaml
kubectl get svc

# Scaling
kubectl scale deployment tic-tac-toe --replicas=5

# Cleanup
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
eksctl delete cluster --name game-cluster --region ap-south-1
```

---

# 🎯 What I Learned

Through this project, I gained practical experience with:

* Docker image creation and container management
* Docker Hub image repositories
* AWS IAM and AWS CLI
* Amazon EKS cluster creation
* Kubernetes Deployments
* Kubernetes Pods
* Kubernetes Services
* AWS LoadBalancer
* Kubernetes scaling
* Application monitoring
* Container resource requests and limits
* Kubernetes troubleshooting
* Cloud application deployment

---

# 💼 DevOps Skills Demonstrated

```text
AWS
├── IAM
├── EKS
└── Load Balancer

Docker
├── Dockerfile
├── Images
├── Containers
└── Docker Hub

Kubernetes
├── Deployment
├── Pods
├── Service
├── Scaling
└── Monitoring

CLI
├── AWS CLI
├── kubectl
└── eksctl
```

---

# Output
<img width="1366" height="768" alt="{3844E3F5-EC80-43C0-9794-DB366D0A1A2F}" src="https://github.com/user-attachments/assets/140a3982-73af-4b65-b2b5-f2326dbcb2f8" />

<img width="1366" height="768" alt="{3CA4682C-D742-4365-A6FA-62583B548961}" src="https://github.com/user-attachments/assets/c354b6e0-3820-438d-b93f-9d6dfa780417" />

<img width="1366" height="768" alt="{3C2C40BA-4D68-4E96-A7E9-C2C8D16B582D}" src="https://github.com/user-attachments/assets/e1e8dd5f-153e-49b9-a28e-5f5c794d50c8" />

<img width="1366" height="768" alt="{B77B46E3-0F2C-4A5F-8C1C-5A3592B06ED7}" src="https://github.com/user-attachments/assets/d414ec69-cd39-47f8-b02f-27a308401f3e" />

<img width="1366" height="768" alt="{2AB64787-C6B4-41CA-9519-E78180065D4E}" src="https://github.com/user-attachments/assets/950825b4-6428-45ab-b36d-be0263203865" />

# 👨‍💻 Author

## Aswad Gorivale

**Cloud / DevOps Enthusiast**

Interested in:

* ☁️ Cloud Computing
* 🚀 DevOps
* 🐳 Docker
* ☸️ Kubernetes
* ☁️ AWS
* 🔄 CI/CD
* 🏗️ Infrastructure as Code

---

# ⭐ Support

If you found this project useful, please consider giving the repository a ⭐ **Star**.

---
