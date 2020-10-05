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
                    args '--entrypoint="" -w ${WORKSPACE}:/data -w /data'
                }
            }
            steps {
                sh "env"
                sh "bash /home/entrypoint.sh debricked:scan $DEBRICKED_CREDENTIALS_USR $DEBRICKED_CREDENTIALS_PSW jenkinsrepo_manual jenkinscommit null cli"
            }
        }
    }
}
