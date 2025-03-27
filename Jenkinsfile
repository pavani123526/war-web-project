pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'sonarqube-server'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/pavani123526/war-web-project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    sh '''
                    mvn sonar:sonar \
                        -Dsonar.projectKey=mahesh-keyy_pavani \
                        -Dsonar.organization=mahesh \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=$SONARQUBE_CREDENTIALS
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


