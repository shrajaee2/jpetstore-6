pipeline {
  agent any
  stages {
    stage('Pre_bulding') {
      steps {
        echo 'Prebuild stage started...'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }

    stage('Sonar Test') {
      parallel {
        stage('Sonar Test') {
          steps {
            sh 'mvn sonar:sonar -Dlicense.skip=true'
          }
        }

        stage('Print test cred') {
          steps {
            echo "the tester is ${TESTER}"
          }
        }

        stage('Print Build Number') {
          steps {
            echo "this is build number ${BUILD_ID}"
          }
        }

      }
    }

    stage('arti') {
      steps {
        script {
          def server = Artifactory.server "artifactory"


          def buildInfo = Artifactory.newBuildInfo()
          def rtMaven = Artifactory.newMavenBuild()

          rtMaven.tool = 'maven'
          rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

          buildInfo = rtMaven.run pom: 'pom.xml', goals: "clean install -Dlicense.skip=true"
          buildInfo.env.capture = true
          buildInfo.name = 'jpetstore-6'
          server.publishBuildInfo buildInfo
        }

      }
    }

    stage('deploy prompt') {
      steps {
        input 'Deploy to production?'
      }
    }

    stage('Deployment') {
      steps {
        echo 'Deploy on production server'
      }
    }

  }
}