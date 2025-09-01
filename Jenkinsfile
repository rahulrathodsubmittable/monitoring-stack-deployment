pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig-id')
    }

    stages {
        stage('Check Cluster') {
            steps {
                sh 'kubectl config current-context'
                sh 'kubectl get nodes -o wide'
            }
        }
    }
}
