pipeline {
    agent none

    environment {
        KUBE_FILE = 'service.yaml'
        SERVICE_NAME = 'bbcnews'
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
                sh '''kubectl delete -f ${KUBE_FILE} || true
                kubectl apply -f ${KUBE_FILE}'''
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
