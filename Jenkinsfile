pipeline {
    agent any

    environment {
        HAB_NOCOLORING = true
        HAB_ORIGIN = 'indellient'
    }

    stages {
        stage('build') {
            steps {
              dir("${workspace}") {
                habitat task: 'build', directory: '.', origin: env.HAB_ORIGIN
              }
            }
        }
        stage('upload') {
            steps {
                withCredentials([string(credentialsId: 'depot-token', variable: 'HAB_AUTH_TOKEN')]) {
                    dir("${workspace}") {
                        habitat task: 'upload', directory: ".", authToken: env.HAB_AUTH_TOKEN
                    }
                }
            }
        }
    }
}
