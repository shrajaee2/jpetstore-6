pipeline {
  agent any
  stages {
    stage('Pre_bulding') {
      steps {
        echo 'Prebuild stage started...'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }

  }
}