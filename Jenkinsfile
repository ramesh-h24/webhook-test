pipeline {
    agent any
    environment {
        MAIN_BRANCH = 'main'
    }
    parameters {
        choice(name: 'TARGET_BRANCH', choices: ['main'], description: 'merging branch')
    }
    
    stages {
        stage('Handle Pull Request Action') {
            steps {
                script {
                    sh "env"
                    def prAction = env.action ?: 'unknown'
                    def REMOTE_GIT_URL = env.GIT_URL
                    echo "Pull request action: ${prAction}"
                    echo " Remote repo url: ${REMOTE_GIT_URL}"
                    
                    if (prAction == 'opened' || prAction == 'reopened' || prAction == 'synchronize') {
                        echo "Handling pull request action: ${prAction}"
                    } else {
                        echo "No specific action needed for pull request action: ${prAction}"
                    }
                }
            }
        }
        stage('Checkout') {
            when {
                expression {
                    prAction == 'opened' || prAction == 'reopened' || prAction == 'synchronize'
                }
            }
            steps {
                checkout scmGit(branches: [[name: '*/master'], [name: ':^(?!origin/patch$|origin/feaure$).*']], extensions: [], userRemoteConfigs: [[credentialsId: 'git_hub', url: 'https://github.com/ramesh-h24/webhook-test.git']])
            }
        }
        stage('Check Merge Conflicts') {
            steps {
                script {
                    def mergeBase = sh(script: 'git merge-base HEAD main', returnStdout: true).trim()
                    def conflictedFiles = sh(script: "git --no-pager diff --name-only --diff-filter=U ${mergeBase} HEAD", returnStatus: true).trim()
                    if (conflictedFiles) {
                        error "Merge conflicts found in files:\n${conflictedFiles}"
                    } else {
                        echo 'No merge conflicts found.'
                    }
                }
            }
        }
        stage('Merge to Main') {
            when {
                allOf {
                    expression {
                        params.TARGET_BRANCH == 'main'
                    }
                    not {
                        branch 'main'
                    }
                    condition {
                        currentBuild.result == null || currentBuild.result == 'SUCCESS'
                    }
                }
            }
            steps {
                script {
                    def mergeMessage = "Merge pull request #${env.CHANGE_ID} from ${env.CHANGE_AUTHOR}"
                    sh "git merge --no-ff -m '${mergeMessage}' ${env.CHANGE_BRANCH}"
                    sh "git push origin ${params.TARGET_BRANCH}"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Build successful - Performing cleanup steps'
            // Add cleanup steps here
        }
        failure {
            echo 'Build failed - Sending notifications'
            // Add notification steps here
        }
    }
}
