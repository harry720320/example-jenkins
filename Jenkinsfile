pipeline {
    agent none

    stages {
        stage('Node.js tests') {
            agent {
                docker {
                    image 'node:14'
                }
            }
            steps {
                sh "npm install"
                sh "npm test"
                sh "node index.js"
            }
        }

        stage('Vulnerability scan') {
            environment {
                DEBRICKED_CREDENTIALS = credentials('debricked-creds')
            }

            agent {
                docker {
                    image 'debricked/debricked-cli'
                    args '--entrypoint="" -v ${WORKSPACE}:/data -w /data'
                }
            }
            steps {
                sh 'bash /home/entrypoint.sh debricked:scan "$DEBRICKED_CREDENTIALS_USR" "$DEBRICKED_CREDENTIALS_PSW" example-jenkins "$GIT_COMMIT" null cli'
            }
        }
    }
}
