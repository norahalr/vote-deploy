node {
    def app

    stage('Clone repository') {
        checkout scm
        sh 'ls -la'
        sh 'cat vote-ui-deployment.yaml'  // ✅ Moved here to avoid subshell issue
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'eeganlf-github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh 'git config user.email "norahnsrr@gmail.com"'
                    sh 'git config user.name "norahalr"'
                    
                    // Modify the file
                    sh 'sed -i "s+norahns/vote.*+norahns/vote:${DOCKERTAG}+g" vote-ui-deployment.yaml'

                    // Confirm the change
                    sh 'cat vote-ui-deployment.yaml'

                    // Git commit and push
                    sh 'git add vote-ui-deployment.yaml'
                    sh 'git commit -m "Done by Jenkins Job deployment: ${env.BUILD_NUMBER}" || echo "No changes to commit"'
                    sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/vote-deploy.git HEAD:main'
                }
            }
        }
    }
}
