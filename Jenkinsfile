
pipeline {
    agent any

    environment {
        REGISTRY = 'localhost:5000'
        IMAGE_NAME = 'student-demo-app'
        PYTHON = 'C:\\Users\\STUDENT\\AppData\\Local\\Programs\\Python\\Python313\\python.exe'
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                echo 'Verifying Python installation...'

                bat '"%PYTHON%" --version'

                echo 'Verifying Python syntax...'

                bat '"%PYTHON%" -m py_compile app.py'

                echo 'Python syntax verification successful!'
            }
        }

        stage('3. Build & Tag Docker Image') {
            steps {
                echo 'Building Docker image...'

                bat '''
                    docker build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% -t %REGISTRY%/%IMAGE_NAME%:latest .
                '''

                echo 'Docker image built successfully!'
            }
        }

        stage('4. Push Image to Registry') {
            steps {
                echo 'Pushing versioned Docker image...'

                bat 'docker push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'

                echo 'Pushing latest Docker image...'

                bat 'docker push %REGISTRY%/%IMAGE_NAME%:latest'

                echo 'Docker images pushed successfully!'
            }
        }
    }

    post {

        success {
            echo '===================================='
            echo 'PIPELINE EXECUTED SUCCESSFULLY!'
            echo "Image: localhost:5000/student-demo-app:${BUILD_NUMBER}"
            echo '===================================='
        }

        failure {
            echo '===================================='
            echo 'PIPELINE FAILED!'
            echo 'Check Jenkins Console Output.'
            echo '===================================='
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
