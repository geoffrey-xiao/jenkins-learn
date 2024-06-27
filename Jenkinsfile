pipeline {
    agent {
      node {
        label 'docker-agent-python'
      }
    }

    stages {
        stage('build') {
            steps {
                echo 'Hello World'
            }
        }

         stage('test') {
            steps {
                echo 'testing'
            }
        }

         stage('deploy') {
            steps {
                echo 'deploy'
            }
        }
    }
}