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
        stage('Checkout') {
            steps {
				git branch: 'main',
				url: 'ssh://git@gitea:22/hannes/osl_cut_off_automation.git',
				credentialsId: 'ssh__gitea_jenkins'

                sh '''
                    echo "Current directory:"
                    pwd
                    echo "Repository content:"
                    ls -la
					echo "aaa"
                '''
            }
        }

    }
}