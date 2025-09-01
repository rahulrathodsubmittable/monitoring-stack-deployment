pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig-id')   // Jenkins credential ID for kubeconfig file
    }

    stages {
        stage('Check Pods') {
            steps {
                sh '''
                  echo "=== Checking running pods in default namespace ==="
                  kubectl get pods -n default
                '''
            }
        }

        stage('Check Configs') {
            steps {
                sh '''
                  echo "=== Checking Grafana Config ==="
                  kubectl get configmap grafana -n default -o yaml || echo "Grafana config not found"

                  echo "=== Checking Prometheus Config ==="
                  kubectl get configmap prometheus-server -n default -o yaml || echo "Prometheus config not found"

                  echo "=== Checking Alertmanager Config ==="
                  kubectl get configmap alertmanager -n default -o yaml || echo "Alertmanager config not found"
                '''
            }
        }

        stage('Get Service URLs') {
            steps {
                sh '''
                  echo "=== Getting Services in default namespace ==="
                  kubectl get svc -n default

                  echo "Grafana URL:"
                  kubectl get svc grafana -n default -o jsonpath="{.spec.ports[0].nodePort}" || echo "Grafana service not found"

                  echo "Prometheus URL:"
                  kubectl get svc prometheus-server -n default -o jsonpath="{.spec.ports[0].nodePort}" || echo "Prometheus service not found"

                  echo "Alertmanager URL:"
                  kubectl get svc alertmanager -n default -o jsonpath="{.spec.ports[0].nodePort}" || echo "Alertmanager service not found"
                '''
            }
        }
    }
}
