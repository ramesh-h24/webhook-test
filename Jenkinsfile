pipeline {
    agent any
    stages {
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
        // Add more stages as needed
    }
}
