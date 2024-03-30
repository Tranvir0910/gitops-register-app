pipeline {
    agent { label "Jenkins-Agent" }
    environment {
        APP_NAME = "register-app"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'ci-register-app', url: 'https://github.com/Tranvir0910/gitops-register-app'
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
                   git config --global user.name "Tranvix0910"
                   git config --global user.email "vitran63666@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'ci-register-app', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Tranvir0910/gitops-register-app main"
                }
            }
        }
      
    }
}
