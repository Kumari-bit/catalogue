pipeline {
    agent {
        label 'roboshop'
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {
        stage('Read Package.json') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    def appVersion = packageJson.version

                    echo "package version: ${appVersion}"
                }
            }
        }

        stage('Install dependencies') {
            steps {
                script {
                   sh """
                      'npm install'
                   """
            }
        }
    }

    post {
        always {
            echo 'I will always say Hello again!'
            deleteDir()
        }

        success {
            echo 'Hello Success'
        }

        failure {
            echo 'Hello Failure'
        }
    }
}