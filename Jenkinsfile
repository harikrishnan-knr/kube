pipeline {
    agent none

    environment {
        KUBE_FILE = 'service.yaml'
        POD_NAME = 'apple-pod'
    }

    stages {
        stage('Checkout for Kubernetes') {
            agent {
                label 'kube'
            }
            steps {
                git branch: 'main', url: 'https://github.com/harikrishnan-knr/kube.git'
            }
        }

        stage('Kubernetes Version Check') {
            agent {
                label 'kube'
            }
            steps {
                sh 'kubectl version --client'
                sh 'eksctl version'
            }
        }

        stage('Verify Kubernetes Files') {
            agent {
                label 'kube'
            }
            steps {
                sh 'ls -la'
            }
        }

        stage('Deploy to Kubernetes') {
            agent {
                label 'kube'
            }
            steps {
                sh 'kubectl apply -f ${POD_NAME}'
            }
        }

        stage('Check Pods') {
            agent {
                label 'kube'
            }
            steps {
                sh '''kubectl get pods -o wide
                kubectl get svc
                kubectl get deployments
                kubectl describe svc ${SERVICE_NAME}'''
            }
        }
    }
}
