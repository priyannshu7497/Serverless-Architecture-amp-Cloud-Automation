📚 Table of Contents
#	Assignment	Status
1	AWS Lambda + CloudWatch Schedule + Alarm	🟢 Completed
2	Dockerize MERN & Deploy using Docker Compose on EC2	🟢 Completed
3	Local Dockerization of MERN Application	🟢 Completed
✅ Assignment 1: AWS Lambda + CloudWatch Schedule + Alarm
🎯 Objective

Create a scheduled Lambda execution pipeline with monitoring and alerts.

🛠 AWS Services Used
Service	Purpose
Lambda	Execute serverless function
EventBridge	Schedule execution
CloudWatch Logs	Store logs
CloudWatch Alarm	Alert on failures
IAM	Assign permissions
📌 Steps
📍 Step 1: Lambda Function Code
def lambda_handler(event, context):
    print("Lambda Triggered Successfully!")
    return {"status": "Success"}


📎 Screenshot → screenshots/lambda_function.png

📍 Step 2: IAM Role

Permissions:
✔ AWSLambdaBasicExecutionRole

📎 screenshots/iam_role.png

📍 Step 3: EventBridge Rule

⏱ Rate: 5 minutes

📎 Screenshot → screenshots/eventbridge_rule.png

📍 Step 4: Logs Verification

📎 Screenshot → screenshots/cloudwatch_logs.png

📍 Step 5: CloudWatch Alarm

Metric: Errors > 0

📎 Screenshot → screenshots/alarm.png

🧠 Architecture Diagram
EventBridge (Cron)
        |
        v
   AWS Lambda
        |
        v
CloudWatch Logs --> CloudWatch Metrics --> Alarm --> SNS (Optional)

🎉 Output

✔ Automatic execution every 5 minutes
✔ Logging + monitoring enabled

🐳 Assignment 2: Deploy MERN Stack using Docker Compose on EC2
🎯 Goal

Deploy a microservices-based MERN stack with Docker containers inside AWS EC2.

⚙️ Tech Stack
Component	Technology
Compute	EC2 Ubuntu
Orchestration	Docker Compose
Backend	Node.js + Express
Frontend	React
Database	MongoDB
📌 Deployment Steps
📍 Step 1: EC2 Setup

Open Ports: 22, 3000, 5000-5002, 27017

📎 Screenshot → screenshots/ec2_instance.png

📍 Step 2: Install Docker
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable --now docker

📍 Step 3: Install Docker Compose
sudo apt install docker-compose -y

📍 Step 4: Clone Repository
git clone https://github.com/<your-fork>/SampleMERNwithMicroservices
cd SampleMERNwithMicroservices

📍 Step 5: Add Dockerfiles

📍 Backend & Profile:

FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]


📍 Frontend:

FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

📍 Step 6: docker-compose.yml
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

📍 Step 7: Build + Run
sudo docker-compose build
sudo docker-compose up -d


📎 Screenshot → screenshots/containers_running.png

🧠 Architecture Diagram
 ┌────────────────────────────────┐
 |           AWS EC2              |
 |--------------------------------|
 | MongoDB | Backend APIs | Frontend |
 | (Docker Containers via Compose) |
 └────────────────────────────────┘

🎉 Output

🚀 App accessible at:

http://EC2_PUBLIC_IP:3000

🧪 Assignment 3: Local Docker Testing
📍 Build Image
docker build -t mern-app .

📍 Run Container
docker run -p 3000:3000 mern-app


📎 Screenshot → screenshots/local_run.png

🏁 Summary Table
Assignment	Status	Result
Lambda Automation	✔ Done	Working & monitored
Docker Compose Deployment	✔ Done	Running on EC2
Local Docker Test	✔ Done	Verified successfully
👨‍💻 Author

📌 Priyanshu Gupta
🎓 Amity University
💼 Cloud | DevOps | AWS | Docker | MERN | CI/CD
