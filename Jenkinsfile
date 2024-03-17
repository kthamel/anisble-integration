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
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m ping
                '''
            }
        }

        stage('Gathering_Facts') {
            steps {
                sh '''
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m gather_facts
                '''
            }
        }

        stage('Ad_Hoc_Command') {
            steps {
                sh '''
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m dnf -a update_cache=true
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
                input id: 'InputMsg', message: 'Are you sure to do that?'
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY playbooks/playbook-fedora-os-update.yaml -v
                '''
            }
        }

    }
}