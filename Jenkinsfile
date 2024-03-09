pipeline {
    agent {label 'ansible'}
    environment {
        ANSIBLE_KEY=credentials('ANSIBLE_KEY')
    }
    stages {
        stage('Versions'){
            steps {
                sh '''
                    ansible --version
                    ansible-playbook --version
                    ansible-galaxy --version
                    ansible-lint --version
                '''
            }
        }

        stage('Syntax_validation') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/playbook-fedora-os-update.yaml --syntax-check 
                '''
            }
        }

        stage('Execution') {
            steps {
                input id: 'InputMsg', message: 'Are you sure to do that?'
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/playbook-fedora-os-update.yaml -v
                '''
            }
        }
    }
}
