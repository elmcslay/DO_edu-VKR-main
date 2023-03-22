pipeline {
    agent any

    stages {
        stage('get project') {
            steps {
                git 'https://github.com/elmcslay/DO_edu-VKR.git'
            }
        }

        stage('initialize tf & plan build node') {
            steps {
                sh 'terraform -chdir=tf_configs/build_cfg/ init'
                sh 'terraform -chdir=tf_configs/build_cfg/ plan'
            }
        }

        stage('make build node') {
            steps {
                sh 'terraform -chdir=tf_configs/build_cfg/ apply -auto-approve'
            }
        }

        stage('initialize tf & plan prod node') {
            steps {
                sh 'terraform -chdir=tf_configs/prod_cfg/ init'
                sh 'terraform -chdir=tf_configs/prod_cfg/ plan'
            }
        }

        stage('make prod node') {
            steps {
                sh 'terraform -chdir=tf_configs/prod_cfg/ apply -auto-approve'
            }
        }

        stage('create environment on nodes') {
            steps {
                sh 'ansible-playbook -i ~/ans_inv/hosts --user=ubuntu --private-key=~/.ssh/id_rsa Playbook.yml'
            }
        }
    }
}