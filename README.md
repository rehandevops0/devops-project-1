This project demonstrates a complete CI/CD pipeline where code is automatically built, containerized, and deployed to a Kubernetes cluster using Jenkins.

📌 Project Architecture
GitHub → Jenkins → Docker → DockerHub → Kubernetes → Web Application

🏗️ Technologies Used

GitHub – Source code management

Jenkins – CI/CD automation

Docker – Build container image

DockerHub – Image registry

Kubernetes (kubeadm cluster) – Deployment & service

Ubuntu EC2 – Servers for Jenkins, Master & Worker node

📁 Project File Structure
devops-project-1/
│
├── Jenkinsfile
├── Dockerfile
├── index.html
└── k8s/
     ├── deployment.yaml
     └── service.yaml

🔧 1. Jenkins Pipeline (Jenkinsfile)

The Jenkinsfile performs:

Checkout repo

Build Docker image

Push image to DockerHub

Deploy to Kubernetes using kubectl

🐳 2. Dockerfile

Used to build an NGINX web application image.

☸️ 3. Kubernetes Manifests
deployment.yaml

Deploys your web app using Docker image:

rehandevops/webapp-cicd:latest

service.yaml

Exposes the application using NodePort.

🚀 4. Steps to Run the Project
A. Setup Jenkins Server
sudo apt update
sudo apt install openjdk-17-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo apt-add-repository "deb https://pkg.jenkins.io/debian binary/"
sudo apt update
sudo apt install jenkins -y


Install plugins:

Docker

Docker Pipeline

Kubernetes CLI

Git

Credentials Binding

B. Configure Docker & DockerHub

Login to DockerHub:

docker login


Create credentials in Jenkins:

ID: dockerhub

Username + Password

C. Connect Jenkins with Kubernetes Cluster

Copy kubeconfig to Jenkins server:

mkdir -p ~/.kube
scp ubuntu@<master-ip>:/home/ubuntu/.kube/config ~/.kube/

D. Run the Pipeline

From Jenkins:

Create Pipeline Job

Set SCM: GitHub repo URL

Build → Pipeline automatically:

Builds Docker image

Pushes to DockerHub

Deploys to Kubernetes

🌐 5. Access Your Web App

Get service details:

kubectl get svc


Open your browser:

http://<worker-node-public-ip>:<node-port>

🔍 Troubleshooting
❌ ImagePullBackOff

Check image name in deployment.yaml:

rehandevops/webapp-cicd:latest


Restart rollout:

kubectl rollout restart deployment webapp

❌ deployment.yaml not found

Run commands from k8s folder:

cd devops-project-1/k8s
kubectl apply -f deployment.yaml

🎯 Project Outcome

✔ Fully automated CI/CD pipeline
✔ Docker image automatically built
✔ Image pushed to DockerHub
✔ Kubernetes deployment updated
✔ Web app successfully accessible



