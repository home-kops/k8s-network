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
        withCredentials(
          [
            string(credentialsId: 'server1-domain', variable: 'DOMAIN'),
            string(credentialsId: 'lets-encrypt-email', variable: 'LETS_ENCRYPT_EMAIL')
          ]
        ) {
          // Run the deploy script dry-run mode to validate the resources
          sh './tooling/deploy -d'
        }
      }
    }
  }
}
