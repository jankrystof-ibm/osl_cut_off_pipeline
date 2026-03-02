pipeline {
    agent any

    stages {
//        stage('Docker login & pull') {
//            steps {
//                withCredentials([usernamePassword(
//                    credentialsId: 'IDENTITY_AUTOMATION_QUAY__1_USRPSWD',
//                    usernameVariable: 'DOCKER_USER',
//                    passwordVariable: 'DOCKER_PASS'
//                )]) {
//                    sh '''
//                        echo "$DOCKER_PASS" | docker login quay.io -u "$DOCKER_USER" --password-stdin
//                        docker pull quay.io/rh-ee-jkrystof/automation_osl_cutoff
//                    '''
//                }
//            }
//        }

        stage('Prepare Git - OSL cut-off') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'IDENTITY_AUTOMATION_QUAY__1_USRPSWD',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        ls -la
                        git clone $OSL_CUT_OFF_AUTOMATION_URL
                        echo konec
                        ls -la $(basename $OSL_CUT_OFF_AUTOMATION_URL)
                    '''
                }
            }
        }
    } // stages

//
//        stage('Prepare Git - OSL cut-off') {
//            steps {
//                withCredentials([sshUserPrivateKey(
//                    credentialsId: 'IDENTITY_AUTOMATION_GITHUB_1_PK',
//                    keyFileVariable: 'SSH_KEY'
//                )]) {
//                    sh '''
//                        TMP_SSH_DIR=$(mktemp -d "$WORKSPACE/ssh_temp.XXXX")
//                        chmod 700 "$TMP_SSH_DIR"
//
//                        # copy ssh private key
//                        cp "$SSH_KEY" "$TMP_SSH_DIR/id_rsa"
//                        chmod 600 "$TMP_SSH_DIR/id_rsa"
//
//                        ssh-keyscan github.com >> "$TMP_SSH_DIR/known_hosts" 2>/dev/null
//
//                        export GIT_SSH_COMMAND="ssh -i $TMP_SSH_DIR/id_rsa -o UserKnownHostsFile=$TMP_SSH_DIR/known_hosts -o StrictHostKeyChecking=yes"
//
//                        git config --global user.name "$IDENTITY_AUTOMATION_GITHUB_1__NAME"
//                        git config --global user.email "$IDENTITY_AUTOMATION_GITHUB_1__EMAIL"
//
//                        git clone $OSL_CUT_OFF_AUTOMATION_URL
//
//                        # rm -rf "$TMP_SSH_DIR"
//                    '''
//                }
//            }
//        }
//    }
//
    post {
        always {
            deleteDir()
        }
    }
}