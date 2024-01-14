pipeline {
    agent any
    environment {
          APP_NAME = "reddit-clone-pipeline"
          DOCKER_USER = "onyima101"
          IMAGE_TAG = "${env.BUILD_NUMBER}"
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
        // stage("Update the Deployment Tags") {
        //     steps {
        //         sh """
        //             cat deployment.yaml
        //             sed -i 's/${APP_NAME}.*/${APP_NAME}:${DOCKERTAG}/g' deployment.yaml
        //             cat deployment.yaml
        //         """
        //     }
        // }
        stage("Push the changed deployment file to GitHub"){
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email onyima_101@yahoo.com"
                            sh "git config user.name onyima101"
                            sh "cat deployment.yaml"
                            sh "sed -i 's+${DOCKER_USER}/${APP_NAME}.*+${DOCKER_USER}/${APP_NAME}:${IMAGE_TAG}+g' deployment.yaml"
                            sh "cat deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/a-reddit-clone-gitops.git HEAD:main"

                        }
                    }
                }
            }
        }

        //  stage("Push the changed deployment file to GitHub") {
        //     steps {
        //         sh """
        //             git config --global user.name "onyima101"
        //             git config --global user.email "onyima_101@yahoo.com"
        //             git add deployment.yaml
        //             git commit -m "Updated Deployment Manifest: ${env.BUILD_NUMBER}'"
        //         """
        //         withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
        //             sh "git push https://github.com/onyima101/a-reddit-clone-gitops HEAD:main"
        //         }
        //     }
        //  }
    }
}
