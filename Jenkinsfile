pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-cicd-app"
        CONTAINER_NAME = "flask-app"
    }

    stages {

        stage('Install Dependencies') {
            steps {
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                bat 'docker stop %CONTAINER_NAME% || exit 0'
                bat 'docker rm %CONTAINER_NAME% || exit 0'
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                docker run -d ^
                -p 5000:5000 ^
                --name %CONTAINER_NAME% ^
                %IMAGE_NAME%
                '''
            }
        }
    }

    post {

        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Pipeline Failed!'
        }
    }
}
