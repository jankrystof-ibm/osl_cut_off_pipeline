pipeline {
    agent any

    stages {
        stage('Login into remote container image registry') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'OSLCUTOFF_IDENTITY_AUTOMATION_QUAY__1_USRPSWD',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        CONTAINER_IMAGE_REGISTRY_HOST=$(echo "$OSL_CUT_OFF_AUTOMATION_CONTAINER_IMAGE" | awk -F/ '{print $1}')
                        echo doing docker login into $CONTAINER_IMAGE_REGISTRY_HOST ...
                        echo "$DOCKER_PASS" | docker login $CONTAINER_IMAGE_REGISTRY_HOST -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Prepare Git - OSL cut-off') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'OSLCUTOFF_IDENTITY_AUTOMATION_GITHUB_1_PK',
                    keyFileVariable: 'SSH_KEY'
                )]) {
                    sh '''

                        git clone $OSL_CUT_OFF_AUTOMATION_URL
                        OSL_CUT_OFF_AUTOMATION_REPOSITORY_DIR_NAME=$(echo $OSL_CUT_OFF_AUTOMATION_URL | sed 's/.git$//' | xargs basename)

                        cp "$SSH_KEY" ./id_rsa

                        export SSH_KEY_LOCATION="$(pwd)/id_rsa"
                        export CONTAINER_IMAGE="$OSL_CUT_OFF_AUTOMATION_CONTAINER_IMAGE"
                        export CLONE_OUT_HOST="$WORKSPACE/OUT"
                        export GIT_USER_NAME=IDENTITY_AUTOMATION_GITHUB_1__NAME
                        export GIT_USER_EMAIL=IDENTITY_AUTOMATION_GITHUB_1__EMAIL
                        mkdir $CLONE_OUT_HOST


                        $OSL_CUT_OFF_AUTOMATION_REPOSITORY_DIR_NAME/run_container.sh

                    '''
                }
            }
        }
    } // stages



    post {
        always {
            deleteDir()
        }
    }

}