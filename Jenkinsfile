
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    environment {
        REGISTRY = 'localhost:5000'
        IMAGE_NAME = 'student-demo-app'

        PYTHON = 'C:\\Users\\STUDENT\\AppData\\Local\\Programs\\Python\\Python313\\python.exe'

        DOCKER = 'C:\\Users\\STUDENT\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
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
                echo 'Checking Python installation...'
                bat '"%PYTHON%" --version'

                echo 'Verifying Python syntax...'
                bat '"%PYTHON%" -m py_compile app.py'

                echo 'Python syntax verification successful!'
            }
        }

        stage('3. Check Docker Installation') {
            steps {
                echo 'Checking Docker version...'
                bat '"%DOCKER%" --version'

                echo 'Checking Docker engine...'
                bat '"%DOCKER%" info'
            }
        }

        stage('4. Build & Tag Docker Image') {
            steps {
                echo 'Building Docker image...'

                bat '''
                    "%DOCKER%" build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% -t %REGISTRY%/%IMAGE_NAME%:latest .
                '''

                echo 'Docker image built successfully!'
            }
        }

        stage('5. Push Image to Registry') {
            steps {
                echo 'Pushing versioned image...'

                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'

                echo 'Pushing latest image...'

                bat '"%DOCKER%" push %REGISTRY%/%IMAGE_NAME%:latest'

                echo 'Images pushed successfully!'
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
            echo 'PIPELINE FAILED!'
            echo 'Check Jenkins Console Output.'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
