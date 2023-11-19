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

    stage('SAST - Escaneamento com Sonarqube'){
      environment {
        // Referencia do Scanner do Sonarqube
        scanner = tool 'sonar-scanner'
      }
      steps{
        // Referencia ao Plugin do Sonarqube
        withSonarQubeEnv('sonarqube') {
          // Execução do scanner com os parametros do Sonarqube
          sh "${scanner}/bin/sonar-scanner -Dsonar.projectKey=$NAME_APP -Dsonar.sources=${WORKSPACE}/ -Dsonar.projectVersion=${BUILD_NUMBER} -Dsonar.dependencyCheck.xmlReportPath=${WORKSPACE}/dependency-check-report.xml -Dsonar.dependencyCheck.htmlReportPath=${WORKSPACE}/dependency-check-report.html"
        }
        // Validação da Qualidade de Código
        timeout(time: 15, unit: ‘MINUTES’)
        waitForQualityGate abortPipeline: true
      }
    }

//    stage('SCA - Dependency Check Scan'){
//     steps{
        //Execução do escaneamento de dependencias
//        dependencyCheck additionalArguments: 'scan="${WORKSPACE}/" --format ALL',
//        odcInstallation: 'dependency-check'
//      }
//    }
  
//    stage('SCA - Dependency Check Publish Report'){
//      steps{
        //Publicação do relatorio de vulnerabilidades de dependencias no Jenkins
//        dependencyCheckPublisher pattern: "dependency-check-report.xml"
//      }
//    }
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
