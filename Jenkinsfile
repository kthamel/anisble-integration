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
                withCredentials([vaultString(credentialsId: 'ansible_key', variable: 'ANSIBLE_KEY')]) {
                    sh  '''
                        echo Key is There!!
                    '''
                } 
            }
        }

        stage('Syntax_Validation') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY.PEM playbooks/playbook-fedora-os-update.yaml --syntax-check 
                '''
            }
        }

        stage('Playbook_Execution') {
            steps {
                withCredentials([vaultString(credentialsId: 'ansible_key', variable: 'ANSIBLE_KEY')]) {
                    input id: 'InputMsg', message: 'Are you sure to do that?'
                    sh '''
                        ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY.PEM playbooks/playbook-fedora-os-update.yaml -v
                    '''
                }
            }
        }
    }
}
