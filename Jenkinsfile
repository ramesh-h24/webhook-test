pipeline {
    agent any
    stages {
        stage('Handle Pull Request Action') {
            steps {
                script {
                    def prAction = $action ?: 'unknown'
                    
                    if (prAction == 'opened') {
                        echo "Pull request was opened"
                        // Perform actions specific to opened pull request
                    } else if (prAction == 'reopened') {
                        echo "Pull request was reopened"
                        // Perform actions specific to reopened pull request
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
