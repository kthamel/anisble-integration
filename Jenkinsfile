pipeline {
    agent {label 'ansible'}
    environment {
        ANSIBLE_SSH_KEY=credentials('ANSIBLE_SSH_KEY')
    }
    stages {
        stage('Check Versions'){
            steps {
                sh '''
                    ansible --version
                    ansible-playbook --version
                    ansible-galaxy --version
                    ansible-lint --version
                '''
            }
        }

        stage('Connectivity Test') {
            steps {
                sh '''
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m ping
                '''
            }
        }

        stage('Ansible Lint') {
            steps {
                sh '''
                    ansible-lint RnD/playbooks/
                '''
            }
        }

        stage('Executing Ad-Hoc Commands') {
            steps {
                sh '''
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m gather_facts
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m gather_facts --limit m2-jenair.39.local
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m dnf -a update_cache=true
                    ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m dnf -a "name=httpd state=present" --become-user=root --become        
                    '''
            }
        }
        stage('Executing Playbooks') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY playbooks/playbook-fedora-os-update.yaml --syntax-check 
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY playbooks/playbook-fedora-os-update.yaml -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_01.yaml --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_02.yaml --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_03.yaml --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_04.yaml --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_05.yaml --tags red --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_05.yaml --tags "red,blue" --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_06.yaml --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_07.yaml --become-user=root --become -v
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY RnD/playbooks/playbook_08.yaml --become-user=root --become -v
                '''
            }
        }
    }
}
