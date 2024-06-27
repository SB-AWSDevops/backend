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

   stages {
    stage('Read the version'){
      steps{
        script{
        def packageJson = readJSON file: 'dir/input.json'
        def appVersion = packageJson.version
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

         stage('check installation') { 
          steps {
            sh """
                npm --version 
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

