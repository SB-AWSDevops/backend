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

    environment{
      def appVersion = ''
    }

   stages {
    stage('Read the version'){
      steps{
        script{
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
                zip -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
            """
            }
        }
    

   }

      post{
        always{
            echo 'I will always say hello again'
            deleteDir()
        }
        success{
            echo 'I will say hello only when success'
        }
        failure{
            echo 'I will say hello only when failure'
        }
      }
    }

