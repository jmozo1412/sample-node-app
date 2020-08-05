pipeline {
    agent any

    environment {
        HAB_LICENSE    = 'accept-no-persist'
        HAB_AUTH_TOKEN = credentials('hab-depot-token')
        HAB_BLDR_URL   = "https://bldr.habitat.sh"
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
                sh "hab pkg build ."
            }
        }
        stage('upload') {
            steps {
                sh "source results/last_build.env && hab pkg upload results/\$pkg_artifact"
            }
        }
        stage('promote') {
            steps {
                sh "source results/last_build.env && hab pkg promote \$pkg_ident stable"
            }
        }
    }
}
