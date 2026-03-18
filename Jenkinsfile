pipeline {
    agent any

    environment {
        DOCKERHUB = "your-dockerhub-username"
        IMAGE = "cicd-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/<your-repo>.git'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $DOCKERHUB/$IMAGE:latest .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker push $DOCKERHUB/$IMAGE:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl rollout restart deployment cicd-app'
            }
        }
    }
}
