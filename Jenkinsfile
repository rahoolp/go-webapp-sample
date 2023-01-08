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
                echo 'Run unit tests if applicable.'
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

        // stage('Raise PR') {
        //     steps {
        //         sh "bash pr.sh"
        //     }
        // }         
    }
}