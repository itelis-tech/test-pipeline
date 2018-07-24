pipeline {
    agent none
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:8-alpine'
                    args '-p 3000:3000'
                }
            }
            steps {
                echo ">>>> Build for Test"
                sh 'npm install'
            }
        }
        stage('All Tests') {
            parallel{
                stage('Quality scan') {
                    steps {
                        echo 'scan'
                    }
                }
                stage('NPM Tests') {
                    agent {
                        docker {
                            image 'node:8-alpine'
                            args '-p 3000:3000'
                        }
                    }
                    parallel {
                        stage("Unit tests") {
                            steps{ sh 'npm run test' }
                        }
                        stage("EtoE Tests") {
                            steps { sh 'npm run test:e2e' }
                        }
                        stage("Coverage test") {
                            steps { sh 'npm run test:cov'}
                        }
                    }
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
