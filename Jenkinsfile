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
                sh '''
                pip install -r requirements.txt
                pytest -v
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE:latest ."
            }
        }

        stage('Login & Push to DockerHub') {
            steps {
                sh "echo Manav@2205 | docker login -u manavmain --password-stdin"
                sh "docker push $IMAGE:latest"
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}

