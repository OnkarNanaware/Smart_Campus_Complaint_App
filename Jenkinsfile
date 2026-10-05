pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/OnkarNanaware/Smart_Campus_Complaint_App.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'python -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t campus-app:jenkins .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker rm -f campus-demo || exit 0'
                bat 'docker run -d --name campus-demo -p 5001:5000 campus-app:jenkins'
            }
        }

        stage('Verify Application') {
            steps {
                bat 'curl http://localhost:5001/health'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the stage logs.'
        }
    }
}