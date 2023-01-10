pipeline {
    agent any
     
    environment {
        GO111MODULE='on'
        NAME='go-webapp-sample'
        VERSION = "${env.BUILD_ID}-${env.GIT_COMMIT}"
        IMAGE_REGISTRY = "rahoolp"
        //IMAGE_REPO = "web-app"
        dockerhub=credentials('dockerhub')
    }

    stages {
        stage('Dev') {
            steps {
                echo 'Dev stage : Run unit tests if applicable.'
                sh 'go test ./...'
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${NAME} ."
                sh "docker tag ${NAME}:latest ${IMAGE_REGISTRY}/${NAME}:${VERSION}"
            }
        }

        stage('Push Image') {
            steps {
                //sh 'docker push ${IMAGE_REGISTRY}/${IMAGE_REPO}/${NAME}:${VERSION}'
                sh 'docker push ${IMAGE_REGISTRY}/${NAME}:${VERSION}'
            }
        }

        // stage('Clone/Pull Repo') {
        //     steps {
        //         script {
        //             if (fileExists('gitops-argocd')) {

        //                 echo 'Cloned repo already exists - Pulling latest changes'

        //                 dir("gitops-argocd") {
        //                     sh 'git remote set-url origin https://github.com/rahoolp/go-webapp-sample'
        //                     sh 'git pull'
        //                 }
        //             } else {
        //                 echo 'Repo does not exists - Cloning the repo'
        //                 sh 'git clone -b feature https://github.com/rahoolp/go-webapp-sample'
        //             }
        //         }
        //     }
        // }
        
        stage('Update Manifest') {
            steps {
                dir("gitops-argocd") {
                    sh 'sed -i "s/"REPLACE"/${NAME}:${VERSION}/" deployment/go-webapp-deployment.yaml'
                    sh 'cat deployment/go-webapp-deployment.yaml'
                }
            }
        }        

        stage('Raise PR') {
            steps {
                sh "gh pr create --base master --head feature"
            }
        }         
    }
}