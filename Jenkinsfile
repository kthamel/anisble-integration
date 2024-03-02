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
                '''
            }
        }

        stage('Validation') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/test-playbook.yaml --syntax-check 
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/test-playbook.yaml
                '''
            }
        }

        stage('Execution') {
            steps {
                sh '''
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/test-playbook.yaml
                '''
            }
        }
    }
}
