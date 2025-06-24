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
        sh '/usr/local/bin/helm lint ./certmanager'
        sh '/usr/local/bin/helm dependency build ./certmanager/'
        sh '/usr/local/bin/helm template ./certmanager -f ./certmanager/values.yaml'

        sh '/usr/local/bin/kubectl kustomize ./metallb --enable-helm'

        sh '/usr/local/bin/helm template kubernetes-replicator mittwald/kubernetes-replicator -f ./replicator/values.yaml'

        sh '/usr/local/bin/helm lint ./traefik'
        sh '/usr/local/bin/helm dependency build ./traefik/'
        sh '/usr/local/bin/helmhelm template ./traefik -f ./traefik/values.yaml'
      }
    }
  }
}
