
pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies'
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Selenium Tests') {
            steps {
                echo 'Running registration tests'
                bat 'python -m pytest -v tests/test_register.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                bat 'docker build -t seleniumdemo:v1 .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying application'
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
                bat 'kubectl get pods'
                bat 'kubectl get services'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
