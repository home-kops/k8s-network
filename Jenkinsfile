pipeline {
  agent {
    kubernetes {
      label 'agent-s'
    }
  }
  stages {
    stage('Clone') {
      steps {
        git branch: 'main',
          credentialsId: 'github-home-kops-token',
          url: 'https://github.com/home-kops/k8s-network.git'
      }
    }

    stage('Verify') {
      steps {
        container('jnlp') {
          sh 'helm lint ./certmanager'
          sh 'helm dependency build ./certmanager/'
          sh 'helm template ./certmanager -f ./certmanager/values.yaml'

          sh 'kubectl kustomize ./metallb --enable-helm'

          sh 'helm template kubernetes-replicator mittwald/kubernetes-replicator -f ./replicator/values.yaml'

          sh 'helm lint ./traefik'
          sh 'helm dependency build ./traefik/'
          sh 'helm template ./traefik -f ./traefik/values.yaml'
        }
      }
    }
  }
}
