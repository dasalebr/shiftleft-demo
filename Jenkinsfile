pipeline {
    agent any
    stages {
        stage('CloudGuard_Shiftleft_IaC') {
            environment {
                CHKP_CLOUDGUARD_CREDS = credentials(CloudGuard_Credentials)
            }
            agent {
                docker { 
                    image 'checkpoint/shiftleft:latest'
                    args '-v /tmp/:/tmp/'
                }
            }
            steps {
                dir('iac-code') {
                    git branch: '{branch}',
                    url: https://github.com/dasalebr/tf_vuln.git
                }
                sh '''
                    export CHKP_CLOUDGUARD_ID=$CHKP_CLOUDGUARD_CREDS_USR
                    export CHKP_CLOUDGUARD_SECRET=$CHKP_CLOUDGUARD_CREDS_PSW
                    shiftleft iac-assessment -i terraform -p iac-code/terraform-template -r -64 -e 3a99f75d-057f-4f6e-8164-baa9bd0c4d8c
                '''
            }
        }
        stage('CloudGuard_Shiftleft_Code_Scan') {
            environment {
                CHKP_CLOUDGUARD_CREDS = credentials(CloudGuard_Credentials)
            }
            agent {
                docker { 
                    image 'checkpoint/shiftleft:latest'
                    args '-v /tmp/:/tmp/'
                }
            }
            steps {
                dir('code-dir') {
                    git branch: '{branch}',
                    url: https://github.com/dasalebr/goof.git
                }
                sh '''
                    export CHKP_CLOUDGUARD_ID=$CHKP_CLOUDGUARD_CREDS_USR
                    export CHKP_CLOUDGUARD_SECRET=$CHKP_CLOUDGUARD_CREDS_PSW
                    shiftleft code-scan -s code-dir -r 489883 -e 3a99f75d-057f-4f6e-8164-baa9bd0c4d8c
                '''
            }
        }
    }
}
