pipeline {
    agent any

    environment {
        HAB_AUTH_TOKEN = credentials('hab-depot-token')
        HAB_BLDR_URL = "https://bldr.habitat.sh"
    }

    parameters {
        string(
            description: 'The Origin To build Package Under',
            name:        'HAB_ORIGIN'
        )
    }

    stages {
        stage('download-keys') {
            steps {
                sh "hab origin key download ${HAB_ORIGIN} --auth ${HAB_AUTH_TOKEN} --url ${HAB_BLDR_URL}"
                sh "hab origin key download ${HAB_ORIGIN} --auth ${HAB_AUTH_TOKEN} --url ${HAB_BLDR_URL} --secret"
            }
        }
        stage('build') {
            steps {
                habitat task: 'build',
                        directory: '.',
                        origin: HAB_ORIGIN,
                        authToken: HAB_AUTH_TOKEN,
                        bldrUrl: HAB_BLDR_URL
            }
        }
        stage('upload') {
            steps {
                habitat task: 'upload',
                        lastBuildFile: "${workspace}/results/last_build.env",
                        authToken: HAB_AUTH_TOKEN,
                        bldrUrl: HAB_BLDR_URL
            }
        }
        stage('promote') {
            steps {
                habitat task: 'promote',
                        channel: 'stable',
                        lastBuildFile: "${workspace}/results/last_build.env",
                        authToken: HAB_AUTH_TOKEN,
                        bldrUrl: HAB_BLDR_URL
            }
        }
    }
}
