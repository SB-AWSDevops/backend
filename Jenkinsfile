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
      stage('test') { 
          steps {
            sh """
                dnf module disable nodejs -y
                dnf module enable nodejs:20 -y
                dnf install nodejs -y
                npm install
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

