🎮 Deploy 2048 Game on Amazon EKS (Kubernetes End-to-End Project)
📌 Project Overview

This project demonstrates an end-to-end Kubernetes deployment on Amazon EKS (Elastic Kubernetes Service) using the classic 2048 web game application.

The goal of this project is to showcase how a containerized application can be deployed, managed, scaled, and exposed to the internet using Kubernetes on AWS.

This project covers:

Docker containerization

Amazon EKS cluster setup

Kubernetes deployments

Load balancing

Persistent storage

Ingress configuration

Scaling and high availability

🏗 Architecture

User → AWS Load Balancer → Kubernetes Service → Deployment → Pods → Container (2048 Game)

Components used:

Amazon EKS

EC2 Worker Nodes

Kubernetes Deployment

Kubernetes Service (LoadBalancer)

AWS Application Load Balancer

Persistent Volume (EBS)

Namespace isolation

🧰 Prerequisites

Before starting, install the following tools:

1️⃣ AWS CLI

Used to interact with AWS services.

aws --version

Configure AWS credentials

aws configure
2️⃣ kubectl

Command line tool for Kubernetes cluster management.

kubectl version --client
3️⃣ eksctl

Tool used to create and manage EKS clusters.

eksctl version
🚀 Project Deployment Steps
Step 1 — Create EKS Cluster

Create a cluster using eksctl:

eksctl create cluster \
--name 2048-cluster \
--region ap-south-1 \
--nodegroup-name 2048-nodegroup \
--node-type t3.medium \  -c7i-flex.large-----use this 
--nodes 2 \
--nodes-min 1 \
--nodes-max 3 \
--managed

Verify cluster

kubectl get nodes
📂 Kubernetes Resources

This project contains the following Kubernetes resources:

Resource	Description
Namespace	Environment isolation
Deployment	Manages 2048 application pods
Service	Exposes application
Ingress	Advanced routing using ALB
PV	Persistent storage
PVC	Storage claim
📁 Project Structure
2048-EKS-Project
│
├── namespace.yaml
├── deployment.yaml
├── service.yaml
├── ingress.yaml
├── pv.yaml
├── pvc.yaml
└── README.md
📦 Create Namespace
apiVersion: v1
kind: Namespace
metadata:
  name: dev-namespace

Apply:

kubectl apply -f namespace.yaml
🐳 Deployment (2048 Game)

This deployment runs 2 replicas of the game container.

apiVersion: apps/v1
kind: Deployment
metadata:
  name: 2048-deployment
  namespace: dev-namespace
spec:
  replicas: 2

Apply:

kubectl apply -f deployment.yaml

Check pods

kubectl get pods -n dev-namespace
💾 Persistent Storage

We use AWS EBS volume as a persistent volume.

Persistent Volume
kind: PersistentVolume
Persistent Volume Claim
kind: PersistentVolumeClaim

Apply:

kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
🌐 Expose Application (Service)

Service type LoadBalancer creates an AWS Elastic Load Balancer automatically.

kind: Service
type: LoadBalancer

Apply:

kubectl apply -f service.yaml

Check service

kubectl get svc -n dev-namespace

Once the EXTERNAL-IP appears, open it in browser.

🚦 Ingress Configuration

Ingress allows advanced routing using AWS ALB.

apiVersion: networking.k8s.io/v1
kind: Ingress

Apply:

kubectl apply -f ingress.yaml
📈 Scaling Application

Increase replicas:

kubectl scale deployment 2048-deployment --replicas=5 -n dev-namespace

Check scaling

kubectl get pods -n dev-namespace
🔍 Useful Commands

Check nodes

kubectl get nodes

Check pods

kubectl get pods -n dev-namespace

Describe service

kubectl describe svc service-2048 -n dev-namespace

Delete resources

kubectl delete -f .

Delete cluster

eksctl delete cluster --name 2048-cluster --region ap-south-1
🎯 Key Learnings

Through this project we learned:

Kubernetes architecture

Deploying applications on Amazon EKS

Working with Pods, Deployments, Services

Implementing persistent storage

Using AWS Load Balancer

Scaling applications in Kubernetes

Managing Kubernetes namespaces

📷 Demo

After deployment, open the Load Balancer DNS in browser.

You will see the 2048 game running on Kubernetes 🚀

👨‍💻 Author

Rutwik Satpute

AWS & DevOps Engineer
Experience: AWS | Kubernetes | Terraform | Linux | CI/CD
