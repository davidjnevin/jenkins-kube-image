pipeline {
    agent {
        kubernetes {
            label 'echo-server-pod'
        }
    }
    stages {
        stage('Create Pod') {
            steps {
                sh 'kubectl apply -f pod.yaml'
            }
        }
        stage('Wait for Pod to be Ready') {
            steps {
                sh 'kubectl wait --for=condition=Ready pod/echo-server-pod --timeout=300s'
            }
        }
        stage('Test Pod') {
            steps {
                sh """
                  kubectl exec -it echo-server-pod -- curl --version
                  kubectl exec -it echo-server-pod -- jq --version
                  kubectl exec -it echo-server-pod -- curl http://localhost:3333/param?query=demo | jq
                """
            }
        }
        stage('Delete Pod') {
            steps {
                sh 'kubectl delete pod echo-server-pod'
            }
        }
    }
}
