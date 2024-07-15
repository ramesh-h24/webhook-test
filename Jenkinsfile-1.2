pipeline {
    agent any
    
    environment {
        MAIN_BRANCH = 'main'
        FEATURE_BRANCHES = ['feature', 'ramesh-h24-patch-1', 'ramesh-h24-patch-2', 'ramesh-h24-patch-3', 'ramesh-h24-patch-4', 'ramesh-h24-patch-5']
        REMOTE_GIT_URL = env.GIT_URL
    }
    
    stages {
        stage('Handle Pull Request Action') {
            steps {
                script {
                    def prAction = env.action ?: 'unknown'
                    echo "Pull request action: ${prAction}"
                    echo "Remote repo url: ${REMOTE_GIT_URL}"
                    
                    if (prAction == 'opened' || prAction == 'reopened' || prAction == 'synchronize') {
                        echo "Checking out main branch: ${MAIN_BRANCH}"
                        checkout scm
                        
                        echo "Checking out pull request branch: ${env.CHANGE_BRANCH}"
                        checkout([$class: 'GitSCM', branches: [[name: "refs/pull/${env.CHANGE_ID}/head"]], userRemoteConfigs: [[credentialsId: 'git_hub', url: '${REMOTE_GIT_URL}']]])
                        
                        // Verify if there are merge conflicts
                        def conflictCheck = sh(script: 'git merge-base --is-ancestor HEAD origin/${env.CHANGE_BRANCH}', returnStatus: true)
                        
                        if (conflictCheck != 0) {
                            error "Conflict detected! Pull request cannot be merged."
                        }
                        
                        // Merge the pull request branch into main
                        sh "git merge --no-ff origin/${env.CHANGE_BRANCH}"
                        
                        // Push changes to main branch
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
