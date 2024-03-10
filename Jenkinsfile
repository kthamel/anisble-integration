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
                withCredentials([vaultString(credentialsId: 'ansible_key', variable: 'PRIVATE_KEY')]) {
                    sh  'echo -----BEGIN OPENSSH PRIVATE KEY----- > ANSIBLE_KEY'
                    sh  'echo $PRIVATE_KEY >> ANSIBLE_KEY'
                    sh  'echo -----END OPENSSH PRIVATE KEY---- >> ANSIBLE_KEY'
                    sh  'chmod 0400 ANSIBLE_KEY'
                } 
            }
        }

        stage('Syntax_Validation') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=ANSIBLE_KEY playbooks/playbook-fedora-os-update.yaml --syntax-check 
                '''
            }
        }

        stage('Playbook_Execution') {
            steps {
                input id: 'InputMsg', message: 'Are you sure to do that?'
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=ANSIBLE_KEY playbooks/playbook-fedora-os-update.yaml -v
                '''
            }
        }
    }
}
