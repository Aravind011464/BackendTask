pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'NodeJS 23' 
        SONAR_SCANNER_PATH = '/Users/senthilvelmuthupandy/Downloads/sonar-scanner-6.2.1.4610-macosx-aarch64/bin'
        PATH = "${NODEJS_HOME}/bin:${SONAR_SCANNER_PATH}:${env.PATH}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm 
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install' 
            }
        }

        stage('Build') {
            steps {
                sh 'npm run start' 
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') 
            }
            steps {
                sh '''
                echo "Starting SonarQube analysis..."
                sonar-scanner -Dsonar.projectKey=BackendTask \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.login=$SONAR_TOKEN
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