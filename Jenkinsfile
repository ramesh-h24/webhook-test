pipeline {
    agent any
    environment {
        MAIN_BRANCH = 'main'
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
                        echo "Checking out main branch: ${MAIN_BRANCH}"
                        checkout scmGit(branches: [[name: 'refs/heads/${MAIN_BRANCH}']], extensions: [], userRemoteConfigs: [[credentialsId: 'git_hub', url: '${REMOTE_GIT_URL}']])
                        

                        def conflictCheck = sh(script: 'git merge-base --is-ancestor HEAD origin/${env.CHANGE_BRANCH}', returnStatus: true)
                        
                        if (conflictCheck != 0) {
                            error "Conflict detected! Pull request cannot be merged."
                        }
                        
                        sh "git merge --no-ff origin/${env.CHANGE_BRANCH}"
                        

                        sh "git push origin ${MAIN_BRANCH}"
                        
                        echo "PR successfully merged with ${MAIN_BRANCH} branch."
                    } else if (prAction == 'closed') {
                        echo "Pull request was closed"
                    } else {
                        echo "Unknown pull request action: ${prAction}"
                    }
                }
            }
        }
    }
}
