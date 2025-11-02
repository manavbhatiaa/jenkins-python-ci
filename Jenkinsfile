pipeline {
    agent any

    environment {
        IMAGE = "manavmain/jenkins-python-ci"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/manavbhatiaa/jenkins-python-ci.git', branch: 'main'
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
                withCredentials([usernamePassword(credentialsId: 'dockerhub_creds', usernameVariable: 'DOCKERHUB_USR', passwordVariable: 'DOCKERHUB_PSW')]) {
                    sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin"
                    sh "docker push $IMAGE:latest"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed! Check logs."
        }
    }
}








































