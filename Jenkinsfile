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
            environment {
                SSH_KEY = credentials('IDENTITY_AUTOMATION_GITHUB_1_PK')
            }
            steps {
                sh '''
                echo WORKSPACE = $WORKSPACE
                ls $WORKSPACE

//                    xcp "$SSH_KEY" ~/.ssh/id_rsa
//                    chmod 600 ~/.ssh/id_rsa
//
//                    mkdir -p ~/.ssh
//                    echo "$SSH_KEY" > ~/.ssh/id_rsa
//                    echo aaaaaa
//                    cat ~/.ssh/id_rsa
//                    chmod 600 ~/.ssh/id_rsa
//                    ssh-keyscan github.com >> ~/.ssh/known_hosts 2>/dev/null
//
//                    git config --global user.name "$IDENTITY_AUTOMATION_GITHUB_1__NAME"
//                    git config --global user.email "$IDENTITY_AUTOMATION_GITHUB_1__EMAIL"
//
//                    git clone git@github.com:jankrystof-ibm/osl_cut_off_automation.git

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