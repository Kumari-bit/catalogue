pipeline {
    agent {
        label 'roboshop'
    }

    environment {
        APPVERSION = ''
        REGION = 'us-east-1'
        ACC_ID = '270370108786'
        PROJECT = 'roboshop'
        COMPONENT = 'catalogue'
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
                    env.APPVERSION = packageJson.version

                    echo "Package Version: ${env.APPVERSION}"
                }
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    withAWS(credentials: 'aws-creds', region: "${REGION}") {

                        sh """
                            aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com

                            docker build -t ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${APPVERSION} .

                            docker push ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${APPVERSION}
                        """
                    }
                }
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