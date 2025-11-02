pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root'   // allows installing packages if needed
        }
    }

    environment {
        IMAGE = "manavmain/jenkins-python-ci"   // your docker image name
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/manavbhatiaa/jenkins-python-ci.git', branch: 'main'
            }
        }

        stage('Install Dependencies & Run Tests') {
            steps {
                sh 'pip install --upgrade pip'
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
                withCredentials([usernamePassword(credentialsId: 'dockerhub_creds', manavmain: 'DOCKER_USER', Manav@2205: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $manavmain --password-Manav@2205'
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
            echo "❌ Pipeline failed!"
        }
    }
}

