pipeline {
  agent any
  stages {
    stage('Stage1') {
      steps {
        echo 'Stage 1 / Step 1'
        echo 'Stage 1 / Step 2'
        echo 'Stage 1 / Step 3'
      }
    }

    stage('Stage 2') {
      steps {
        echo 'Stage 2 / Step 1'
        echo 'Stage 2 / Step 2'
        sh 'echo ${TESTE3}'
      }
    }

  }
  environment {
    TESTE = '1'
    TESTE2 = '12'
    TESTE3 = '123'
  }
}