pipeline {
    agent any

    stages {

        stage('1. Check Cluster Status') {
            steps {
                bat 'kubectl cluster-info'
                bat 'kubectl get nodes'
            }
        }

        stage('2. Create Nginx Pod') {
            steps {
                bat 'kubectl run nginx-pod --image=nginx --restart=Never --dry-run=client -o yaml | kubectl apply -f -'
            }
        }

        stage('3. List Pods') {
            steps {
                bat 'kubectl get pods'
            }
        }

        stage('4. Describe Pod') {
            steps {
                bat 'kubectl describe pod nginx-pod'
            }
        }

        stage('5. View Pod Logs') {
            steps {
                bat 'kubectl wait --for=condition=Ready pod/nginx-pod --timeout=60s'
                bat 'kubectl logs nginx-pod'
            }
        }

        stage('6. Create Deployment') {
            steps {
                bat 'kubectl create deployment nginx-deploy --image=nginx --dry-run=client -o yaml | kubectl apply -f -'
            }
        }

        stage('7. Scale Deployment') {
            steps {
                bat 'kubectl scale deployment nginx-deploy --replicas=3'
                bat 'kubectl get pods'
            }
        }

        stage('8. Expose Deployment as Service') {
            steps {
                bat 'kubectl expose deployment nginx-deploy --port=80 --type=NodePort --dry-run=client -o yaml | kubectl apply -f -'
                bat 'kubectl get services'
            }
        }

        stage('9. Delete Resources') {
            steps {
                bat 'kubectl delete pod nginx-pod --ignore-not-found=true'
                bat 'kubectl delete deployment nginx-deploy --ignore-not-found=true'
                bat 'kubectl delete service nginx-deploy --ignore-not-found=true'
            }
        }

    }

    post {
        success {
            echo 'All Kubernetes steps completed successfully!'
        }
        failure {
            echo 'Kubernetes pipeline failed. Check logs.'
        }
    }
}
