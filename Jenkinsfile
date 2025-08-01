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
           git branch: 'main', credentialsId: 'git-ssh', url: 'git@github.com:mdang-dev/unify-gitops.git'
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
  stage('Prepare SSH') {
     steps {
          sh """
            mkdir -p ~/.ssh
            ssh-keyscan github.com >> ~/.ssh/known_hosts || true
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
             sshagent(credentials: ['git-ssh']) {
              sh 'git push git@github.com:mdang-dev/unify-gitops.git main'
            }
      }
  }
}
}
