node {
    def app

     stage('Clone repository') {
         checkout scm
     }

     stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'gitops', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh "git config --global user.email master@code-lab.kr"
                    sh "git config --global user.name codelab-kr"
                    sh "git checkout feature"
                    sh "git pull"
                    sh "sed -i 's/ssung-test:.*\$/ssung-test:${IMAGETAG}/g' flask-example-deploy/deployment.yaml"
                    sh "cat ./scripts/deploy/${SERVICE}.yaml"
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job updatemaifest: ${IMAGETAG}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/flask-example-apps.git HEAD:feature"
                }
            }
        }
     }
}
