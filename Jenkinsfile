pipeline{
    agent{
        label "Jenkins-Agent"
    }

    environment{
        APP_NAME = "register-app-pipeline"
    }
    stages{
        stage("Cleanup WS"){
            steps{
                cleanWs()
            }
          
        
        }

        
        stage("chkout SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/pranaygitac/argomanifest'
            }
        }

         stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "pranaygitac"
                   git config --global user.email " pranaysingapore57@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/pranaygitac/argomanifest main"
                }
            }
    }
    
}
}