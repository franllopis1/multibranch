pipeline {
  agent any
  stages {
    stage('Buzz build') {
      parallel {
        stage('Build Java 7') {
          steps {
            stash(name: 'Buzz Java 7', includes: 'target/**')
          }
        }

        stage('Build Java 8') {
          steps {
            stash(name: 'Buzz Java 8', includes: 'target/**')
          }
        }

      }
    }

    stage('Buzz test') {
      parallel {
        stage('Testing A 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Buzz Java 7'
          }
        }

        stage('Testing B 7') {
          steps {
            unstash 'Buzz Java 8'
          }
        }

        stage('Testing A 8') {
          steps {
            unstash 'Buzz Java 8'
          }
        }

        stage('Testing B 8') {
          steps {
            unstash 'Buzz Java 7'
          }
        }

      }
    }

    stage('Confirm Deploy to Staging') {
      steps {
        input(message: 'Deploy to Stage', ok: 'Hazlo')
      }
    }

    stage('Deploy to Staging') {
      agent {
        node {
          label 'java8'
        }

      }
      steps {
        unstash 'Buzz Java 8'
        sh './jenkins/deploy.sh staging'
      }
    }

  }
}