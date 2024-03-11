pipeline {
    agent {label 'ansible'}
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
                withCredentials([vaultFile(credentialsId: 'ANSIBLE_SSH_KEY', variable: 'ANSIBLE_SSH_KEY')]) {
                    sh  '''
                        echo Key is There!!
                    '''
                } 
            }
        }

        stage('Syntax_Validation') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY playbooks/playbook-fedora-os-update.yaml --syntax-check 
                '''
            }
        }

        stage('Playbook_Execution') {
            steps {
                withCredentials([vaultFile(credentialsId: 'ANSIBLE_SSH_KEY', variable: 'ANSIBLE_SSH_KEY')]) {
                    input id: 'InputMsg', message: 'Are you sure to do that?'
                    sh '''
                        ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY playbooks/playbook-fedora-os-update.yaml -v
                    '''
                }
            }
        }
    }
}
