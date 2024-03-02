pipeline {
    agent {label 'ansible'}
    environment {
        ANSIBLE_KEY=credentials('ANSIBLE_KEY')
    }
    stages {
        stage('versions'){
            steps {
                sh '''
                    ansible --version
                    ansible-playbook --version
                    ansible-galaxy --version
                '''
            }
        }

        stage('validation')
            steps {
                sh '''
                    ansible-playbook -i playbooks/hosts --private-key=$ANSIBLE_KEY playbooks/test-playbook.yaml --syntax-check 
                    ansible-playbook -i playbooks/hosts --private-key=$ANSIBLE_KEY playbooks/test-playbook.yaml --vv
                '''
            }
    }
}