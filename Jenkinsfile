pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "doc201/flask-app"
        TAG = "${BUILD_NUMBER}"
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', credentialsId: 'github-creds', url: 'https://github.com/Dev664/test_cicd.git'
            }
        }

        stage('Build Image') {
            steps {
                sh '''
                docker build -t $DOCKER_IMAGE:$TAG .
                '''
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $DOCKER_IMAGE:$TAG
                    '''
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh '''
                kubectl set image deployment/flask-deployment \
                flask=$DOCKER_IMAGE:$TAG

                kubectl rollout status deployment/flask-deployment
                '''
            }
        }
    }
}
