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
        sh 'helm lint ./certmanager'
        sh '/usr/local/bin/kubectl kustomize ./metallb --enable-helm'
        sh 'helm lint ./replicator'
        sh 'helm lint ./traefik'
      }
    }
  }
}
