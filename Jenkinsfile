pipeline {
    agent any
    environment {
          APP_NAME = "reddit-clone-pipeline"
          GIT_USERNAME = "onyima101"
          GIT_EMAIL = "onyima_101@yahoo.com"
          GIT_REPONAME = "a-reddit-clone-gitops"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/onyima101/a-reddit-clone-gitops'
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
        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "${GIT_USERNAME}"
                    git config --global user.email "${GIT_EMAIL}"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/${GIT_USERNAME}/${GIT_REPONAME} HEAD:main"
                }
            }
        }
    }
}