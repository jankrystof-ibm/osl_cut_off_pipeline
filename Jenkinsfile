pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
				git branch: 'main',
				url: 'ssh://git@gitea:22/hannes/osl_cut_off_automation.git',
				credentialsId: 'gitea-ssh-creds'

                sh '''
                    echo "Current directory:"
                    pwd
                    echo "Repository content:"
                    ls -la
                '''
            }
        }
    }
}