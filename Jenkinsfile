pipeline {
  agent any
  environment { IMAGEN = "gtn-app" }
  stages {
    stage('Validar IaC') {
      steps {
        sh 'cfn-lint --version || pip3 install --break-system-packages cfn-lint'
        sh 'cfn-lint iac/*.yaml'
      }
    }
    stage('Construir imagen') {
      steps { sh 'docker build -t $IMAGEN app/' }
    }
    stage('Desplegar') {
      steps { sh 'cd app && docker compose up -d --build' }
    }
  }
  post {
    success { echo 'Despliegue OK — verificar puerto 80' }
    failure { echo 'Pipeline falló — revisar logs' }
  }
}
