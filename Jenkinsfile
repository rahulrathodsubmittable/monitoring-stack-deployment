pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig-id')  // Replace with your Jenkins kubeconfig credentials ID
    }

    stages {
        stage('List Pods') {
            steps {
                sh '''
                  echo "=== All Running Pods in default namespace ==="
                  kubectl get pods -n default -o wide
                '''
            }
        }

        stage('Check Grafana Config') {
            steps {
                sh '''
                  if kubectl get pods -n default | grep -i grafana; then
                      echo "=== Grafana Pod Found ==="
                      kubectl describe pod -l app=grafana -n default || true
                      echo "=== Grafana Service Info ==="
                      kubectl get svc -l app=grafana -n default -o wide || true
                  else
                      echo "Grafana not found"
                  fi
                '''
            }
        }

        stage('Check Prometheus Config') {
            steps {
                sh '''
                  if kubectl get pods -n default | grep -i prometheus; then
                      echo "=== Prometheus Pod Found ==="
                      kubectl describe pod -l app=prometheus -n default || true
                      echo "=== Prometheus Service Info ==="
                      kubectl get svc -l app=prometheus -n default -o wide || true
                  else
                      echo "Prometheus not found"
                  fi
                '''
            }
        }

        stage('Check Alertmanager Config') {
            steps {
                sh '''
                  if kubectl get pods -n default | grep -i alertmanager; then
                      echo "=== Alertmanager Pod Found ==="
                      kubectl describe pod -l app=alertmanager -n default || true
                      echo "=== Alertmanager Service Info ==="
                      kubectl get svc -l app=alertmanager -n default -o wide || true
                  else
                      echo "Alertmanager not found"
                  fi
                '''
            }
        }

        stage('Print Access URLs') {
            steps {
                sh '''
                  echo "=== Service Endpoints in default namespace ==="
                  kubectl get svc -n default -o wide

                  echo "If NodePort is exposed, you can access via: http://localhost:<NodePort>"
                  echo "If ClusterIP only, you need port-forwarding, e.g.:"
                  echo "kubectl port-forward svc/grafana 3000:3000 -n default"
                '''
            }
        }
    }
}
