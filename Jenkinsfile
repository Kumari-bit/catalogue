pipeline {
    agent  {
        label 'AGENT-1'
    }
    environment { 
        appVersion = ''
    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
    }
    // parameters {
    //     booleanParam(name: 'deploy', defaultValue: false, description: 'Toggle this value')
    // }
    // Build
    stages {
        stage('Read package.json') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Package version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                   sh """
                        npm install
                   """
                }
            }
        }
    }
}