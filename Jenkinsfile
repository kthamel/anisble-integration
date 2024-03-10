pipeline {
    agent {label 'ansible'}
    environment {
        ANSIBLE_KEY=credentials('ANSIBLE_KEY')
    }
    stages {
        stage('Check_Versions'){
            steps {
                sh '''
                    ansible --version
                    ansible-playbook --version
                    ansible-galaxy --version
                    ansible-lint --version
                '''
            }
        }
        
    stage('Invoke_Ansible_Credentials') {
            steps {
                withCredentials([vaultString(credentialsId: 'ansible_key', variable: '')]) {
                    sh 'echo Stage Passed'
                } 
            }
        }

        stage('Syntax_Validation') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/playbook-fedora-os-update.yaml --syntax-check 
                '''
            }
        }

        stage('Playbook_Execution') {
            steps {
                input id: 'InputMsg', message: 'Are you sure to do that?'
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/playbook-fedora-os-update.yaml -v
                '''
            }
        }
    }
}
