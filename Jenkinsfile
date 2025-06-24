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
          sh '''
            helm lint ./certmanager && \
              helm dependency build ./certmanager && \
              helm template ./certmanager -f ./certmanager/values.yaml
          '''

          sh 'kubectl kustomize ./metallb --enable-helm'

          sh '''
            helm repo add mittwald https://helm.mittwald.de && \
              helm repo update && \
              helm template kubernetes-replicator mittwald/kubernetes-replicator -f ./replicator/values.yaml
          '''

          sh '''
            helm lint ./traefik && \
              helm dependency build ./traefik && \
              helm template ./traefik -f ./traefik/values.yaml
          '''
        }
      }
    }
  }
}
