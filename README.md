# ec2-python-docker-deploy
Guide to deploying a Python Flask app on AWS EC2 using Docker

# 🚀 Deploy a Simple Python Flask App on EC2 with Docker


**Purpose:** Step-by-step guide to deploy a Python Flask app on a single EC2 instance using Docker — cost-efficient and beginner-friendly.

---

## ✅ Assumptions

- AWS account with EC2 access
- Region: `us-east-1`
- Instance: Ubuntu 22.04, `t2.micro` (free-tier eligible)
- App: Simple Python Flask app
- Docker Hub account created
- Tools: AWS Console or CLI, SSH key pair

---

## 1️⃣ Launch EC2 Instance

1. Go to [EC2 Dashboard](https://console.aws.amazon.com/ec2)
2. Launch instance with:
   - **Name:**  python-app-server
   - **AMI:** Ubuntu Server 22.04 LTS
   - **Type:**  t2.micro
   - **Key Pair:** Select or create `.pem` key

   - **VPC**: Default or existing or create a VPC
   
   - **Network:** Public subnet, auto-assign public IP enabled
   - **Security Group:**
     - SSH (22): My IP
     - HTTP (80): `0.0.0.0/0`
     - TCP (5000): `0.0.0.0/0`
       
   - **Storage:** 8 GiB (gp3)

---

## 2️⃣ Install Docker on EC2

sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
newgrp docker

- verify
docker --version


## 3️⃣ Create Python Flask App

- create a directory
  
mkdir python-app && cd python-app

1. inside the directory create a file for app
# Create app.py:

  nano app.py

- inside paste the content from app.py
- then save the file with ctrl + o
- press enter to save the file with the name
- now exit with ctrl + x

2. inside the directory create a file for requirements
# Create requirements.txt:

  nano requirements.txt

- inside paste the content from requirements.txt
- then save the file with ctrl + o
- press enter to save the file with the name
- now exit with ctrl + x

3. inside the directory create a file for Dockerfile
# Create Dockerfile:

   nano Dockerfile

- inside paste the content from Dockerfile
- then save the file with ctrl + o
- press enter to save the file with the name
- now exit with ctrl + x


## 4️⃣ Docker Hub Login on EC2 
 
 Log in to Docker Hub 
 
docker login  # Enter username and password or access token

## to create a personal access token
- go to docker hub settings > Personal access tokens > generate new token 

docker login -u <your-username>

- for  password enter the password given in token

## 5️⃣ Build & Push Docker Image on EC2

docker build -t docker.io/<your-username>/python-app:latest .
docker images  # Verify image
docker push docker.io/<your-username>/python-app:latest

# Verify in Docker Hub: hub.docker.com > python-app > latest tag 


## 6️⃣ Run Docker Container

 docker run -d -p 5000:5000 --name python-app docker.io/<your-username>/python-app:latest
 docker ps
 docker logs python-app


## 7️⃣ Test the App

## Open your browser and navigate to:

http://<EC2-PUBLIC-IP>:5000

🔁 Traffic Flow 

Client → EC2 Public IP:5000 → Ubuntu Host → Docker Container → Python Flask App




# Troubleshoot:
# - Check security group allows port 5000
# - Verify container: docker ps
# - Check logs: docker logs python-app

## Clean Up to Avoid Billing

docker stop python-app
docker rm python-app


🎉 You're Done!
You've successfully deployed a Python Flask app on EC2 using Docker.


