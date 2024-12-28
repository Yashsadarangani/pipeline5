pipeline {
    agent any
    environment {
        PATH = "${PATH};C:\\Windows\\System32"
        PYTHON_PATH = 'C:\\Users\\Yashu Kun\\AppData\\Local\\Programs\\Python\\Python312;C:\\Users\\Yashu Kun\\AppData\\Local\\Programs\\Python\\Python312\\Scripts'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Coverage Installation') {
            steps {
                bat '''
                set PATH=%PYTHON_PATH%;%PATH%
                pip show coverage
                '''
            }
        }

        stage('Run Unit Tests and Generate Coverage') {
            steps {
                bat '''
                set PATH=%PYTHON_PATH%;%PATH%
                echo "Running tests with coverage..."
                coverage run --source=. test.py
                coverage xml -o coverage.xml
                if exist coverage.xml (
                    echo "Coverage report generated successfully."
                ) else (
                    echo "Error: Coverage report not found!"
                    exit /b 1
                )
                '''
            }
        }

        stage('Ensure Correct Working Directory') {
            steps {
                bat '''
                set PATH=%PYTHON_PATH%;%PATH%
                echo "Current working directory: %cd%"
                dir
                '''
            }
        }

        stage('SonarQube Analysis') {

            steps {
                bat '''
                set PATH=%PYTHON_PATH%;%PATH%
                sonar-scanner -Dsonar.projectKey=pipeline5 ^
                              -Dsonar.projectName=pipeline5 ^
                              -Dsonar.sources=. ^
                              -Dsonar.python.coverage.reportPaths=coverage.xml ^
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.token=sqa_e0d66921a5e37d4859d748d025d4fe0c23afcbc7
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
