pipeline {
    agent any
    environment {
        MAIN_BRANCH = 'main'
    }
    stages {
        stage('Handle Pull Request Action') {
            steps {
                script {
                    def prAction = env.action ?: 'unknown'
                    echo "Pull request action: ${prAction}"
                    
                    if (prAction == 'opened' || prAction == 'reopened' || prAction == 'synchronize') {
                        echo "Checking out main branch: ${MAIN_BRANCH}"
                        checkout([$class: 'GitSCM', branches: [[name: "refs/heads/${MAIN_BRANCH}"]], userRemoteConfigs: [[url: 'env.GIT_URL']]])
                        

                        def conflictCheck = sh(script: 'git merge-base --is-ancestor HEAD origin/${env.CHANGE_BRANCH}', returnStatus: true)
                        
                        if (conflictCheck != 0) {
                            error "Conflict detected! Pull request cannot be merged."
                        }
                        
                        sh "git merge --no-ff origin/${env.CHANGE_BRANCH}"
                        

                        sh "git push origin ${MAIN_BRANCH}"
                        
                        echo "PR successfully merged with ${MAIN_BRANCH} branch."
                    } else if (prAction == 'closed') {
                        echo "Pull request was closed"
                        // Perform actions specific to closed pull request
                    } else {
                        echo "Unknown pull request action: ${prAction}"
                        // Handle unknown action accordingly
                    }
                }
            }
        }
        // Add more stages as needed
    }
}
