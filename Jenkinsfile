pipeline {
    agent any
    stages {
        stage('Checkout scm') {
            steps {
                checkout scm
            }
        }
        stage('PULL PR from GIt') {
            steps {
                script {
                    echo 'pulling Pr from git...'
                }
            }
        }
        stage('merging to Integration branch test') {
            steps {
                script {
                    echo 'checking merge conflicts | if no conflicts it will merge to integration branch'
                }
            }
        }
        stage('GE testing ') {
            steps {
                script {
                    echo 'testing and validating code'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
