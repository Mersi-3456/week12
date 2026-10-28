
pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Start Flask and Run Tests') {
            steps {
                bat '''
                    start "FlaskApp" /B python app.py
                    timeout /t 5 /nobreak
                    python -m pytest -v tests/test_register.py
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t seleniumdemo:v1 .'
            }
        }

        
stage('Deploy to Kubernetes') {
    steps {
        bat 'kubectl apply -f deployment.yaml'
        bat 'kubectl apply -f service.yaml'
        bat 'kubectl rollout status deployment/seleniumdemo-deployment --timeout=120s'
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
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
