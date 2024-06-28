pipeline {
  agent {
        label 'AGENT-1'
  }

    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }

    environment {
      def appVersion = ''
        nexusUrl = 'nexus.surisetty.online:8081'
    }

  stages {
    stage('Read the version') {
      steps {
        script {
          def packageJson = readJSON file: 'package.json'
          appVersion = packageJson.version
          echo "application version: $appVersion"
        }
      }
    }
      stage('Install Dependencies') {
          steps {
            sh """
                npm install
                ls -ltr
                echo $appVersion
            """
          }
      }

    stage('Build') {
          steps {
            sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
            """
          }
    }

        stage('Nexus Artifact upload') {
          steps {
            script {
              nexusArtifactUploader(
              nexusVersion: 'nexus3',
              protocol: 'http',
              nexusUrl: "${nexusUrl}",
              groupId: 'com.expense',
              version: "${appVersion}",
              repository: "backend",
              credentialsId: 'nexus-auth',
              artifacts: [
                  [artifactId: "backend",
                  classifier: '',
                  file: "backend-"+ "${appVersion}" + '.zip',
                  type: 'zip']
              ]
    )
            }
          }
        }

  }

      post {
        always {
      echo 'I will always say hello again'
      deleteDir()
        }
        success {
      echo 'I will say hello only when success'
        }
        failure {
      echo 'I will say hello only when failure'
        }
      }
}
