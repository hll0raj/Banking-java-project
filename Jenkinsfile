pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url:'https://github.com/hll0raj/Banking-java-project.git'
                echo 'git url checkout'
            }
        }
        stage('Maven Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Maven Checkstyle') {
            steps {
                // Run Maven Checkstyle
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Maven Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Maven Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('run dockerfile and build image') {
            steps {
                sh 'docker build -t rajni644/project:1 .'
            }
        }
        stage('login to docker hub') {
            steps{
                withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpass')]) {
                    sh 'docker login -u rajni644 -p ${dockerhubpass}'
                }                
            }
        }
        stage('push to dockerhub') {
            steps {
                sh ' docker push rajni644/project:1 '
            }
        }
        stage('deployment stage using ansible') {
            steps {
                ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', sudoUser: null, vaultCredentialsId: 'dockerhubpassword', vaultTmpPath: ''
            }
        }
    }
}
