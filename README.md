# 🏥 Patient Management System

A microservices-based Patient Management System built with **Spring Boot**, containerized using **Docker**, and deployed in a simulated AWS cloud environment via **LocalStack**. It includes services for authentication, patient records, billing, and analytics—securely orchestrated through an API Gateway and JWT-based authentication.

---

## 📌 Features

- ✅ Authentication & Authorization (JWT)
- ✅ Patient CRUD operations
- ✅ Billing calculations via gRPC
- ✅ Real-time analytics using Kafka
- ✅ ECS (Fargate) simulation with LocalStack
- ✅ PostgreSQL database via RDS simulation
- ✅ Infrastructure-as-Code (CloudFormation JSON with AWS CDK)

## 🛠️ Setup Instructions
## ✅ Prerequisites

Make sure the following tools are installed on your system:

- [Download Java 24](https://jdk.java.net/24/)

- [Download Maven](https://maven.apache.org/download.cgi)

- [Install Docker](https://docs.docker.com/get-docker/)  

- [Install Localstack](https://www.localstack.cloud/)

 - [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- **Configure AWS CLI for LocalStack**

      aws configure --profile localstack
  
## 📦 Clone the Repository

    git clone https://github.com/your-username/patient-management.git
    cd patient-management

🚀 Start LocalStack

    localstack start


## 🐳 Build Docker Images

    docker build -t auth-service ./auth-service
    docker build -t patient-service ./patient-service
    docker build -t billing-service ./billing-service
    docker build -t analytics-service ./analytics-service
    docker build -t api-gateway ./api-gateway

## 🏗️ Deploy Infrastructure (CloudFormation)

Use the ECS task definitions and services defined in localstack.template.json.
Deploy them using the AWS CLI or any ECS-compatible automation targeting LocalStack.
    
    aws --endpoint-url=http://localhost:4566 cloudformation deploy \
          --template-file localstack.template.json \
          --stack-name patient-management
          
🌐 Access the API Gateway

Once everything is deployed, access the API Gateway endpoint:

    curl http://<API_GATEWAY_DNS_NAME>/auth/register

    You can find the actual Gateway DNS name in the CloudFormation outputs:
    APIGatewayServiceLoadBalancerDNS

---
## 🧱 Architecture Overview

```plaintext
Client
  │
  ▼
[ ALB Load Balancer ]
  │
  ▼
[ API Gateway (Spring Cloud Gateway) ]
  ├── /auth/**       → Auth Service (JWT)
  ├── /patients/**   → Patient Service
  ├── /billing/**    → Billing Service (gRPC)
  └── /analytics/**  → Analytics Service

Supporting Components:
- Kafka (MSK)
- PostgreSQL (RDS)
- AWS Secrets Manager
- CloudWatch Logs
- VPC with Public & Private Subnets

---
