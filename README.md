# Deploy-a-Dockerized-Application-to-EC2-Using-Jenkins

## Project Overview

This project demonstrates how to automate the deployment of a Dockerized application to an AWS EC2 instance using Jenkins CI/CD Pipeline.

The workflow includes:

1. Source Code Management using GitHub
2. Continuous Integration using Jenkins
3. Docker Image Build
4. Docker Hub Image Push
5. Automated Deployment to AWS EC2

---

## Architecture

GitHub → Jenkins → Docker Build → Docker Hub → AWS EC2 → Docker Container

---

## Technologies Used

- AWS EC2
- Jenkins
- Docker
- Docker Hub
- GitHub
- Ubuntu Linux
- Jenkins Pipeline

---

## Project Workflow

### Step 1: Create Application

Develop a simple application and store the source code in a GitHub repository.

### Step 2: Configure Jenkins

- Install Jenkins on Ubuntu.
- Configure required plugins:
  - Git Plugin
  - Docker Pipeline Plugin
  - SSH Agent Plugin

### Step 3: Connect GitHub Repository

Configure Jenkins to pull source code from GitHub.

### Step 4: Build Docker Image

Create a Dockerfile and build the image through Jenkins.

Example:

```bash
docker build -t myapp .
```

### Step 5: Push Image to Docker Hub

Login and push the image.

```bash
docker login
docker push username/myapp:latest
```

### Step 6: Launch AWS EC2 Instance

- Create Ubuntu EC2 instance.
- Allow ports:
  - 22 (SSH)
  - 80 (HTTP)
  - 8080 (Application Port)

### Step 7: Install Docker on EC2

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```

### Step 8: Jenkins Deployment

Jenkins connects to EC2 using SSH and executes:

```bash
docker pull username/myapp:latest
docker stop myapp || true
docker rm myapp || true
docker run -d --name myapp -p 80:80 username/myapp:latest
```
## Jenkins Pipeline

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "adimulam22/my-app:latest"
    }

    stages {
        stage('GitHub') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Sai-Adimulam/Deploy-a-Dockerized-Application-to-EC2-Using-Jenkins'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t my-app .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag my-app $DOCKER_IMAGE'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }
}

