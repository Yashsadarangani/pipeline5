pipeline {
    agent any
    
    environment {
        SONARQUBE_SCANNER = 'sonarscanner' // Name of your SonarQube scanner
        PATH = "${PATH};C:\\Windows\\System32"
    }

    stages {
        stage('Install Dependencies') {
            steps {
              bat 'pip install coverage'
            }
        }

        stage('Run Tests with Coverage') {
            steps {
               bat '''
               coverage run -m unittest discover
               coverage xml -o coverage.xml
              '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') { // Match the server name configured in Jenkins
                    bat '''
                    sonar-scanner.bat ^
                    -Dsonar.projectKey=pipeline5 ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=sqa_e0d66921a5e37d4859d748d025d4fe0c23afcbc7 ^
                    -Dsonar.python.coverage.reportPaths=coverage.xml
                    '''
                }
            }
        }
    }
    
    post {
        always {
            script {
                echo 'Pipeline Completed'
            }
        }
        failure {
            echo 'Pipeline Failed'
        }
        success {
            echo 'Pipeline Succeeded'
        }
    }
}
