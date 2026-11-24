pipeline {
  agent any
  environment { IMAGEN = "gtn-app" }
  stages {
    stage('Validar IaC') {
      steps {
        // pip instala cfn-lint en ~/.local/bin del usuario jenkins, que no está en el PATH
        sh '''
          export PATH="$HOME/.local/bin:$PATH"
          cfn-lint --version || pip3 install --user --break-system-packages cfn-lint
          cfn-lint iac/*.yaml
        '''
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
