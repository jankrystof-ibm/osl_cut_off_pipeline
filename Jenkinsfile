pipeline {
    agent any

    stages {
        stage('Docker login & pull') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'IDENTITY_AUTOMATION_QUAY__1_USRPSWD',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login quay.io -u "$DOCKER_USER" --password-stdin
                        docker pull quay.io/rh-ee-jkrystof/automation_osl_cutoff
                    '''
                }
            }
        }

        stage('Prepare SSH & Git') {
                    steps {
                        sh '''
                            # vytvoření dočasného adresáře pro SSH klíč
                            TMP_SSH_DIR=$(mktemp -d "$WORKSPACE/ssh_temp.XXXX")
                            chmod 700 "$TMP_SSH_DIR"

                            # zkopírování privátního klíče z Jenkins credentials do temp dir
                            cp $IDENTITY_AUTOMATION_GITHUB_1_PK $TMP_SSH_DIR/id_rsa
                            chmod 600 "$TMP_SSH_DIR/id_rsa"

                            # přidání github.com do known_hosts v temp dir
                            ssh-keyscan github.com >> "$TMP_SSH_DIR/known_hosts" 2>/dev/null

                            # export proměnných pro git/ssh
                            export GIT_SSH_COMMAND="ssh -i $TMP_SSH_DIR/id_rsa -o UserKnownHostsFile=$TMP_SSH_DIR/known_hosts -o StrictHostKeyChecking=yes"

                            # git config z environment proměnných buildu
                            git config --global user.name "$IDENTITY_AUTOMATION_GITHUB_1__NAME"
                            git config --global user.email "$IDENTITY_AUTOMATION_GITHUB_1__EMAIL"

                            # klon repo jen pokud složka neexistuje
                            if [ ! -d "$WORKSPACE/osl_cut_off_automation" ]; then
                                git clone git@github.com:jankrystof-ibm/osl_cut_off_automation.git
                            fi

                            # smazání dočasného adresáře
                            rm -rf "$TMP_SSH_DIR"
                        '''
                    }
                }

//        stage('Checkout') {
//            steps {
//				git branch: 'main',
//				url: 'ssh://git@gitea:22/hannes/osl_cut_off_automation.git',
//				credentialsId: 'ssh__gitea_jenkins'
//
//                sh '''
//                  ssh-keyscan github.com >> ~/.ssh/known_hosts
//                  chmod 644 ~/.ssh/known_hosts
//
//                  # Start ssh-agent
//                  eval "$(ssh-agent -s)"
//
//                  # Add key
//                  ssh-add ~/.ssh/id_ed25519
//                  echo "Loaded SSH identities:"
//                  ssh-add -l
//
//                # Setup git identities
//                git config --global user.name "pepa zdepa"
//                git config --global user.email "pepa@z.depa"
//
//                    echo "Current directory:"
//                    pwd
//                    echo "Repository content:"
//                    ls -la
//					echo "aaa"
//                '''
//            }
//        }

    }
}