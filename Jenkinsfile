pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t my-k8s-app:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Start Minikube') {
            steps {
                sh '''
                if ! minikube status | grep -q "Running"; then
                    minikube start --driver=docker
                fi
                '''
            }
        }

        stage('Load Image into Minikube') {
            steps {
                sh '''
                minikube image load my-k8s-app:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                sed -i "s/IMAGE_TAG/${BUILD_NUMBER}/g" k8s/deployment.yaml
                kubectl apply -f k8s/
                '''
            }
        }
    }
}
