pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        go 'go-1.17'
    }    

    enviornment {
        GO111MODULE='on'
        NAME='go-webapp-sample'
        VERSION = "${env.BUILD_ID}-${env.GIT_COMMIT}"
    }

    stages {
        stage('dev') {
            steps {
                sh 'go test ./...'
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${NAME} ."
                sh "docker tag ${NAME}:latest ${IMAGE_REGISTRY}/${IMAGE_REPO}/${NAME}:${VERSION}"
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push ${IMAGE_REGISTRY}/${IMAGE_REPO}/${NAME}:${VERSION}'
            }
        }
    }
}