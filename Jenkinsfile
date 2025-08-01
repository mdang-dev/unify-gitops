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
           git branch: 'main', credentialsId:'github', url: 'https://github.com/mdang-dev/unify-gitops'
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
    stage("Push Changes") {
      steps {
        sshagent(['git-ssh']) {
          sh """
            git config user.name "mdang-dev"
            git config user.email "minhdang25.dev@gmail.com"
            git add deployment.yaml
            git commit -m "Updated Deployment Manifest"
            git push origin main
          """
        }
      }
    }
  }
}
