pipeline {
    agent {label 'ansible'}

    stages {
        stage('versions'){
            steps {
                sh '''
                    ansible --version
                    ansible-playbook --version
                    ansible-galaxy --version
                '''
            }
        }
    }
}