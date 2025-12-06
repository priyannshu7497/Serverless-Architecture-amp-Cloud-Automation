# 🚀 Cloud & DevOps Deployment Portfolio

![Status](https://img.shields.io/badge/STATUS-COMPLETED-brightgreen?style=for-the-badge)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=flat-square&logo=amazonaws)
![Docker](https://img.shields.io/badge/Docker-Container-blue?style=flat-square&logo=docker)
![Node.js](https://img.shields.io/badge/Node.js-Backend-green?style=flat-square&logo=node.js)
![React](https://img.shields.io/badge/React-Frontend-blue?style=flat-square&logo=react)
![Python](https://img.shields.io/badge/Python-Lambda-yellow?style=flat-square&logo=python)

---

## 📚 Overview

This repository contains detailed documentation and artifacts for three completed assignments covering AWS Lambda automation, Dockerization of a MERN application, and deployment using Docker Compose on an EC2 instance.

**Assignments included here:**

1. **AWS Lambda + CloudWatch Schedule + CloudWatch Alarm**  
2. **Dockerize the MERN App and Run Using Docker Compose on EC2**  
3. **Local Dockerizing of the MERN Application**

> **Author:** Priyanshu Gupta — Amity University  
> **Contact:** (gpriyannshu@gmail.com)

---

## 🔖 Table of Contents

- [Assignment Status](#-assignment-status)
- [Assignment 1 — Lambda + Scheduler + Alarm](#-assignment-1--lambda--scheduler--alarm)
  - [Objective](#objective)
  - [Services Used](#services-used)
  - [Implementation Steps](#implementation-steps)
  - [Architecture Diagram](#architecture-diagram)
  - [Result & Validation](#result--validation)
- [Assignment 2 — Dockerize MERN & Deploy on EC2](#-assignment-2--dockerize-mern--deploy-on-ec2)
  - [Objective](#objective-1)
  - [Tech Stack](#tech-stack)
  - [Step-by-step Deployment](#step-by-step-deployment)
  - [Deployment Architecture](#deployment-architecture)
  - [Result & Validation](#result--validation-1)
- [Assignment 3 — Local Dockerization of MERN](#-assignment-3--local-dockerization-of-mern)
- [Folder Structure](#folder-structure)
- [How to Reproduce / Run Locally](#how-to-reproduce--run-locally)
- [Screenshots & Evidence](#screenshots--evidence)
- [Conclusions](#conclusions)
- [Author](#author)
- [Submission Notes](#submission-notes)

---

## ✅ Assignment Status

| # | Assignment | Status |
|---|-----------:|:------:|
| 1 | Lambda + EventBridge + CloudWatch Alarm | ✅ Completed |
| 2 | Dockerize MERN & Docker Compose on EC2 | ✅ Completed |
| 3 | Local Dockerization of MERN | ✅ Completed |

---

## ✅ Assignment 1: AWS Lambda + CloudWatch Schedule + Alarm

### Objective
Create a scheduled Lambda pipeline with monitoring (CloudWatch Logs) and alerting (CloudWatch Alarm).

### Services Used
- AWS Lambda  
- EventBridge (CloudWatch Scheduler / Rule)  
- CloudWatch Logs & Metrics  
- CloudWatch Alarm (Errors metric)  
- IAM (Lambda execution role)

### Implementation Steps

**Step 1 — Create Lambda function**

- Runtime: **Python 3.9**
- Simple function code (example):

```python
def lambda_handler(event, context):
    print("Lambda Triggered Successfully!")
    return {"status": "Success"}

Step 2 — Attach IAM role

    Attach policy: AWSLambdaBasicExecutionRole (allows writing logs).

    For production restrict with least privilege; for assignment AWSLambdaBasicExecutionRole is sufficient.

Step 3 — Create EventBridge Rule (Scheduler)

    Type: Rate or cron expression.

    Example: run every 5 minutes

        rate(5 minutes) or equivalent Cron in scheduler UI

    Add Lambda function as target.

Step 4 — Validate CloudWatch Logs

    Go to CloudWatch → Logs → /aws/lambda/<function-name> and verify log streams and prints.

Step 5 — Create CloudWatch Alarm

    Metric: Errors (Lambda)

    Condition: Errors > 0 for 1 evaluation period (or adjust)

    Add notification action (SNS topic) if required.

Architecture Diagram

EventBridge (Cron / rate)
          ↓
       AWS Lambda
          ↓
   CloudWatch Logs → CloudWatch Metrics
          ↓
     CloudWatch Alarm → (SNS / Email)

Result & Validation

    Lambda executes at configured schedule.

    Logs confirm executions.

    Alarm triggers on error metrics (if simulated).

✅ Assignment 2: Dockerize MERN & Deploy using Docker Compose on EC2
Objective

Containerize MERN microservices and run them on an EC2 instance with Docker Compose.
Tech Stack

    EC2 (Ubuntu 22.04)

    Docker, Docker Compose

    Node.js (Backend)

    React (Frontend)

    MongoDB (Container)

Step-by-step Deployment

1. Launch EC2 (Ubuntu)

    Configure security group to allow:

        22 (SSH)

        3000 (Frontend)

        5000/5001/5002 (Backend services)

        27017 (MongoDB internal — recommended restrict to internal or same host)

2. Install Docker & Docker Compose

sudo apt update
sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo apt install docker-compose -y

3. Clone repository

git clone https://github.com/<your-fork>/SampleMERNwithMicroservices
cd SampleMERNwithMicroservices

4. Add Dockerfiles (example)

Backend / Service Dockerfile (example)

FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]

Frontend Dockerfile (example)

FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

5. Create docker-compose.yml

Example docker-compose.yml:

version: "3.9"

services:
  mongo:
    image: mongo:latest
    container_name: mongo-db
    ports:
      - "27017:27017"

  hello-service:
    build: ./backend/helloService
    ports:
      - "5001:5000"
    depends_on:
      - mongo

  profile-service:
    build: ./backend/profileService
    ports:
      - "5002:5000"
    depends_on:
      - mongo

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - hello-service
      - profile-service

6. Build & run

sudo docker-compose build
sudo docker-compose up -d

7. Verify

    sudo docker ps — check running containers

    Open browser to http://<EC2_PUBLIC_IP>:3000 (frontend)

Deployment Architecture

 ┌──────────────────────────────────────────────┐
 │                    EC2                       │
 │  ┌────────┐  ┌────────────┐  ┌─────────────┐  │
 │  │MongoDB │  │Hello-API   │  │Profile-API  │  │
 │  └────────┘  └────────────┘  └─────────────┘  │
 │                     ┌────────────┐           │
 │                     │ Frontend   │           │
 │                     └────────────┘           │
 └──────────────────────────────────────────────┘

Result & Validation

    MERN microservices running in Docker containers.

    Frontend accessible via EC2 public IP on port 3000.

✅ Assignment 3: Local Dockerization of MERN App

This is the local container build and run process without Compose.

Build:

docker build -t mern-app .

Run:

docker run -p 3000:3000 mern-app

Verify:

    Open http://localhost:3000 (or container host IP)

📁 Folder Structure (recommended)

Project-Repo/
├─ backend/
│  ├─ helloService/
│  │  ├─ Dockerfile
│  │  └─ ...
│  ├─ profileService/
│  │  ├─ Dockerfile
│  │  └─ ...
├─ frontend/
│  ├─ Dockerfile
│  └─ ...
├─ screenshots/
│  ├─ lambda_function.png
│  ├─ iam_role.png
│  ├─ eventbridge_rule.png
│  ├─ cloudwatch_logs.png
│  ├─ alarm.png
│  ├─ ec2_instance.png
│  ├─ docker_installed.png
│  ├─ containers_running.png
│  └─ local_run.png
├─ docker-compose.yml
└─ README.md
```
🖼️ Screenshots & Evidence
Create Function
(<img src="https://raw.githubusercontent.com/Serverless-Architecture-amp-Cloud-Automation/main/1.Create%20Lambda%20Function%20+%20CloudWatch%20Schedule%20+%20CloudWatch%20Alarm/screenshot/1%20creaatet%20function.png" width="600">)

Event Bridge
(<img src="https://raw.githubusercontent.com/Serverless-Architecture-amp-Cloud-Automation/main/1.Create%20Lambda%20Function%20+%20CloudWatch%20Schedule%20+%20CloudWatch%20Alarm/screenshot/lambda%20function%202.png" width="600">)

(<img src="https://raw.githubusercontent.com/Serverless-Architecture-amp-Cloud-Automation/main/1.Create%20Lambda%20Function%20+%20CloudWatch%20Schedule%20+%20CloudWatch%20Alarm/screenshot/lambda%20function%203.png" width="600">)

Set Alarm with Lamda Function

(<img src="https://raw.githubusercontent.com/Serverless-Architecture-amp-Cloud-Automation/main/1.Create%20Lambda%20Function%20+%20CloudWatch%20Schedule%20+%20CloudWatch%20Alarm/screenshot/lambda%20function%20test%20alarm.png" width="600">)

![Lambda function screenshot](screenshots/lambda_function.png)

```
Refer to the placeholders already used above for image names.
🧾 How to Reproduce (quick)

    Lambda: Create Lambda in AWS Console → paste code → create EventBridge scheduled rule → assign IAM role → verify logs and alarm.

    EC2: Launch Ubuntu EC2 → install Docker & Compose → clone repo → build & run with docker-compose.

    Local: From project root run docker build -t mern-app . and docker run -p 3000:3000 mern-app.

✅ Conclusions

    All three assignments are implemented and tested.

    Infrastructure is reproducible using the above steps.

    Logging, monitoring and basic alerting are in place for Lambda.

    MERN app runs reliably in Docker containers on EC2 and locally.

🧑‍💻 Author

Priyanshu Gupta
Cloud / DevOps / MERN / AWS / Docker
