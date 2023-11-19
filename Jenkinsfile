pipeline{
  // Inicio do Pipeline
  agent any
  environment {
    // Definições de variáveis para uso na pipeline
    NAME_APP = "dexter"
  }
  
  // Estágio de clone do repositório
  stages {
    stage('Repositório de Código'){
      steps{
        git credentialsId: 'gitlab',
          url: 'https://github.com/gaelmi/fiap_devops',
          branch: 'main'
        stash 'dexter-repositorio'
      }
    }
  }

  // Execuções de finalização da Pipeline
  post {
    always { chuckNorris() }
    success {
      echo "Pipeline executada com sucesso!"
    }
    failure {
      echo "Pipeline falhou!"
    }
  }
}
