pipeline {
  agent any
  environment {
    KUBECONFIG = '/etc/rancher/k3s/k3s.yaml'
  }
  stages {
    stage('Build image') {
      steps {
        sh 'docker build -t todo:${BUILD_NUMBER} .'
      }
    }
    stage('Load image into k3s') {
      steps {
        sh 'docker save todo:${BUILD_NUMBER} | sudo /usr/local/bin/k3s ctr images import -'
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'sed "s/IMAGE_TAG/${BUILD_NUMBER}/" k8s/deployment.yaml | kubectl apply -f -'
        sh 'kubectl apply -f k8s/service.yaml'
        sh 'kubectl rollout status deployment/todo-app --timeout=120s'
      }
    }
  }
}
