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

