📌 Assignment Status
#	Assignment Name	Status
1	AWS Lambda + CloudWatch Schedule + Alarm	🟢 Completed
2	Dockerized MERN + Docker Compose Deployment on EC2	🟢 Completed
3	Local Dockerization of MERN Application	🟢 Completed
✅ Assignment 1: AWS Lambda + CloudWatch Schedule + Alarm
🎯 Objective

Create a scheduled AWS Lambda function with automated logging and monitoring alerts.

🔧 AWS Services Used
Service	Purpose
AWS Lambda	Execute serverless code
EventBridge Rule	Scheduler (Cron trigger)
CloudWatch Logs	Store Lambda logs
CloudWatch Alarm	Error alert monitoring
IAM Role	Permissions for execution
📌 Implementation Steps
📝 Step 1 — Create Lambda Function

Runtime: Python 3.9

def lambda_handler(event, context):
    print("Lambda Triggered Successfully!")
    return {"status": "Success"}


📎 Screenshot: screenshots/lambda_function.png

🔐 Step 2 — IAM Role Assigned

Role Used: AWSLambdaBasicExecutionRole

📎 Screenshot: screenshots/iam_role.png

⏱ Step 3 — Schedule EventBridge Trigger

Frequency: Every 5 minutes

📎 Screenshot: screenshots/eventbridge_rule.png

📊 Step 4 — Monitor Logs

Logs verified in:
➡️ CloudWatch → Log Groups → /aws/lambda/<function-name>

📎 Screenshot: screenshots/cloudwatch_logs.png

🚨 Step 5 — Setup CloudWatch Alarm

Metric: Errors > 0

📎 Screenshot: screenshots/alarm.png

🧠 Architecture Diagram
 EventBridge (Cron Trigger)
            ↓
        AWS Lambda
            ↓
     CloudWatch Logs
            ↓
     CloudWatch Metrics
            ↓
     Alarm → (SNS Optional)

🎉 Result

✔ Lambda runs automatically every 5 minutes
✔ Alerts set if Lambda fails
✔ Logging + monitoring enabled

🐳 Assignment 2: Deploy Dockerized MERN Stack Using Docker Compose on EC2
🎯 Objective

Containerize and deploy a microservices-based MERN project on AWS EC2 using Docker Compose.

⚙️ Components
Service	Technology
Frontend	React
Backend APIs	Node.js
Database	MongoDB
Deployment	Docker & Docker Compose
Hosting	AWS EC2 (Ubuntu 22.04)
📌 Steps Executed
1️⃣ Launch EC2 Instance

Allowed ports:
22, 3000, 5000–5002, 27017

📎 Screenshot: screenshots/ec2_instance.png

2️⃣ Install Docker
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable --now docker

3️⃣ Install Docker Compose
sudo apt install docker-compose -y

4️⃣ Clone Project
git clone https://github.com/<forked-repo>/SampleMERNwithMicroservices
cd SampleMERNwithMicroservices

5️⃣ Add Dockerfiles

Backend Example:

FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]


Frontend Example similar but exposes 3000.

6️⃣ Create docker-compose.yml
version: "3.9"

services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"

  hello-service:
    build: ./backend/helloService
    ports:
      - "5001:5000"

  profile-service:
    build: ./backend/profileService
    ports:
      - "5002:5000"

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"

7️⃣ Build & Run Containers
sudo docker-compose build
sudo docker-compose up -d


📎 Screenshot: screenshots/containers_running.png

🧠 Architecture Diagram
      ┌─────────────────────────────┐
      │         AWS EC2             │
      │ ┌──────────┐ ┌───────────┐ │
      │ │ Frontend │ │ Backends   │ │
      │ └──────────┘ └───────────┘ │
      │         ┌───────────┐      │
      │         │ MongoDB   │      │
      │         └───────────┘      │
      └─────────────────────────────┘

🎉 Result

✔ MERN app successfully deployed and running at:

👉 http://EC2_PUBLIC_IP:3000

🧪 Assignment 3: Local Dockerization of MERN App
Step	Command
Build Image	docker build -t mern-app .
Run Container	docker run -p 3000:3000 mern-app

📎 Screenshot: screenshots/local_run.png

📁 Folder Reference Structure
📦 Project
 ┣ 📁 screenshots
 ┣ 📄 README.md
 ┣ 📁 backend
 ┣ 📁 frontend
 ┣ 📄 docker-compose.yml

👨‍💻 Author

Priyanshu Gupta
AMITY University
DevOps | AWS | Docker | Cloud Engineer
