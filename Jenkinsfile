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
                '''
            }
        }

        stage('Execution') {
            steps {
                sh '''
                    hostnamectl
                    ansible-playbook -i inventory/hosts --private-key=$ANSIBLE_KEY playbooks/test-playbook.yaml
                '''
            }
        }
    }
}
