pipeline {
    agent any

    stages {
        stage('github') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Sai-Adimulam/Deploy-a-Dockerized-Application-to-EC2-Using-Jenkins'
            }
        }

        stage('Build Docker') {
            steps {
                bat 'docker build -t my-app .'
            }
        }

        stage('Tag Image') {
            steps {
                bat 'docker tag my-app adimulam22/my-app:latest'
            }
        }

        stage('Push Image') {
            steps {
                bat 'docker push adimulam22/my-app:latest'
            }
        }
    }
}
