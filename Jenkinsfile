pipeline {
    agent {label 'ansible'}
    environment {
        ANSIBLE_SSH_KEY=credentials('ANSIBLE_SSH_KEY')
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

        stage('Connectivity_Test') {
            steps {
                sh '''
                    ansible -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m ping
                '''
            }
        }

        stage('Gathering_Facts') {
            steps {
                sh '''
                    ansible -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m gather_facts
                '''
            }
        }

        stage('Gathering_Facts') {
            steps {
                sh '''
                    ansible -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m gather_facts
                '''
            }
        }

        stage('Ad_Hoc_Command_1') {
            steps {
                sh '''
                    ansible -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m dnf -a update_cache=true
                '''
            }
        }

        stage('Ad_Hoc_Command_2') {
            steps {
                sh '''
                    ansible -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m dnf -a "name=httpd state=present" --become-user=root --become        
                '''
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
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY playbooks/playbook-fedora-os-update.yaml -v
                '''
            }
        }

        stage('Playbook_01_Execution') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_01.yaml --become-user=root --become -v
                '''
            }
        }

        stage('Playbook_02_Execution') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_02.yaml --become-user=root --become -v
                '''
            }
        }
    }
}
