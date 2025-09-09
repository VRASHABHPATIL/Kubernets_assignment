
````markdown
# Kubernetes Assignment 🚀

This project demonstrates how to containerize, analyze, and deploy an application on a Kubernetes cluster hosted in **AWS**.  
It also integrates **Nexus Repository (EC2)**, **SonarCloud** for code analysis, and **Prometheus + Grafana** for monitoring.

---

## 📌 Architecture Overview
- **AWS EKS Cluster** – Hosts the Kubernetes workloads  
- **Nexus Repository (EC2)** – Stores build artifacts / Docker images  
- **SonarCloud** – Performs static code analysis and quality checks  
- **Prometheus & Grafana** – Monitors application and cluster health  
- **Application** – Deployed via Kubernetes manifests (`.yml` files)  

---

## ⚙️ Prerequisites
Make sure you have the following installed and configured:

- [AWS CLI](https://docs.aws.amazon.com/cli/)  
- [`kubectl`](https://kubernetes.io/docs/tasks/tools/)  
- [`eksctl`](https://eksctl.io/) (or Terraform/CloudFormation)  
- Docker  
- Access to AWS (IAM user with permissions for EKS, EC2, IAM roles, etc.)  
- SonarCloud account  
- Prometheus configuration files  

Clone this repo:

```bash
git clone https://github.com/VRASHABHPATIL/Kubernets_assignment.git
cd Kubernets_assignment
````

---

## 🚀 Step 1: Create Kubernetes Cluster in AWS

```bash
# Create an EKS cluster using eksctl
eksctl create cluster \
  --name kubernetes-assignment \
  --region ap-south-1 \
  --nodegroup-name linux-nodes \
  --node-type t3.medium \
  --nodes 2

# Verify cluster connection
kubectl get nodes
```

---

## 🚀 Step 2: Setup Nexus Repository on EC2

```bash
# Launch an EC2 instance (Ubuntu/Amazon Linux)
# Install Docker and run Nexus container

sudo yum install docker -y    # or apt-get install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker

docker run -d -p 8081:8081 --name nexus sonatype/nexus3

# Access Nexus at:
# http://<EC2-Public-IP>:8081
# Use Nexus as a private Docker registry or artifact repo
```

---

## 🚀 Step 3: Integrate SonarCloud for Code Analysis

```bash
# 1. Create a project in SonarCloud
# 2. Add the Sonar token in your CI/CD pipeline (GitHub Actions, Jenkins, etc.)
# 3. Run analysis during build to ensure code quality

# Example Maven command:
mvn sonar:sonar \
  -Dsonar.projectKey=<your_project_key> \
  -Dsonar.organization=<your_org> \
  -Dsonar.host.url=https://sonarcloud.io \
  -Dsonar.login=<your_token>
```

---

## 🚀 Step 4: Setup Prometheus Monitoring

```bash
# Deploy Prometheus to Kubernetes
kubectl apply -f prometheus-config.yaml
kubectl apply -f prometheus-deployment.yaml
kubectl apply -f prometheus-service.yaml

# Access Prometheus UI
kubectl port-forward svc/prometheus-service 9090:9090

# Then open in browser:
# http://localhost:9090
```

---

## 🚀 Step 5: Deploy Application to Kubernetes

```bash
# Build and push your Docker image (to Nexus or DockerHub)
docker build -t <nexus-repo-url>/kubernetes-app:latest .
docker push <nexus-repo-url>/kubernetes-app:latest

# Update the deployment.yml to use the pushed image
# Apply the manifests
kubectl apply -f deployment-service.yml
```

---

## 🧹 Cleanup

```bash
# Delete resources
kubectl delete -f deployment.yml
kubectl delete -f service.yml
eksctl delete cluster --name kubernetes-assignment
```

```


Do you also want me to add a **final step for Grafana setup** (since you mentioned Prometheus + Grafana) so that the README looks 100% complete for monitoring setup?
```
