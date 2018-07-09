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
                withCredentials([string(credentialsId: 'depot-token', variable: 'HAB_AUTH_TOKEN')]) {
                    sh "ls ${workspace}/results"
                    habitat task: 'upload', directory: "${workspace}/results", authToken: env.HAB_AUTH_TOKEN
                }
              }
            }
        }
    }
}
