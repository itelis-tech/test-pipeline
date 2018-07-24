pipeline {
    agent {
        docker {
            image 'node:8-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                echo ">>>> Build for Test"
                sh 'npm install'
            }
        }
        stage('Tests') {
            parallel{
                stage('Quality scan') {
                    steps {
                        echo 'scan'
                    }
                }
                stage("Unit tests") {
                    steps{ sh 'npm run test' }
                }
                stage("E2E Tests") {
                    steps { sh 'npm run test:e2e' }
                }
                stage("Coverage test") {
                    steps { sh 'npm run test:cov'}
                }
            }
        }
        stage('Delivery') {
            steps {
                echo ">>>> Delivery"
                sh 'cap ${JOB_BASE_NAME} deploy'
            }
        }
    }
}
