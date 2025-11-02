pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub_creds')
        IMAGE = "yourdockerhubusername/jenkins-python-ci"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/YOUR-USERNAME/jenkins-python-ci.git'
            }
        }

        stage('Install & Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE:latest ."
            }
        }

        stage('Login & Push to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin"
                sh "docker push $IMAGE:latest"
            }
        }
    }
}

