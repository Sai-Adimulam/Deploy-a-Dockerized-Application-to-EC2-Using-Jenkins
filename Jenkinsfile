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
                sh 'docker build -t my-app .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag my-app adimulam22/my-app:latest'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push adimulam22/my-app:latest'
            }
        }
    }
}
