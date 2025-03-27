pipeline {
    agent any
    
    environment {
        SONARQUBE_URL = "http://localhost:9000"  // Update with your SonarQube URL
        SONARQUBE_CREDENTIALS = "sonarqube" // Jenkins credentials ID for SonarQube token
    }

    stages {
        stage('Build') {
            steps {
                echo "Building the project..."
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') { // 'SonarQube' should match Jenkins Global Tool Configuration name
                 sh '''   
                
                  mvn sonar:sonar \
                  -Dsonar.projectKey=pavani123-456_ruthvikanavishna \
                  -Dsonar.organization=pavani123 \
                  -Dsonar.host.url=http://sonarcloud.io\
                  -Dsonar.login=${SONARQUBE_CREDENTIALS}
                 '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}

