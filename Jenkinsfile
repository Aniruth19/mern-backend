pipeline {
    agent any
    tools {
        nodejs 'NodeJS'  
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm  
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'  
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') {  
                    bat 'sonar-scanner.bat -D"sonar.projectKey=backend-project" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.token=sqp_82ae541279bd3b0292b3c1c296e19cc696da8cdd"'  
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
}
