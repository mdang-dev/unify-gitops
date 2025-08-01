pipeline {
  agent any
  environment {
    APP_NAME = "unify-backend"
  }
  stages {
     stage('clean workspace') {
         steps {
            cleanWs()
         }
     }
    stage('Checkout from Git') {
        steps {
           git branch: 'main', credentialsId: 'github', url: 'https://github.com/mdang-dev/unify-gitops'
        }
    }
    stage("Update the Deployment Tags") {
        steps {
             sh """
                cat deployment.yaml
                sed -i "s|image: .*${APP_NAME}:.*|image: minhdangdev/${APP_NAME}:${IMAGE_TAG}|" deployment.yaml
                cat deployment.yaml
              """
        }
  }
  stage("Push the changed deployment file to GitHub") {
        steps {
            sh """
                    git config --global user.name "mdang-dev"
                    git config --global user.email "minhdang25.dev@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
              """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/mdang-dev/unify-gitops main"
                }
        }
    }
  }
}
