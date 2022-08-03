pipeline {
    agent any 
    parameters {
         choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Chose the environment')
         string(name: 'COMPONENT', defaultValue: '', description: 'Enter the name of the component')
    }
    environment { 
        SSH_CRED = credentials('SSH-Cenos7')
    }
    stages {
        stage('Do a dry-run') {
            steps {
                sh "env"   // Just to see tne env variables
                sh "ansible-playbook robot-dryrun.yml -e ansible_user=centos -e ansible_password=DevOps321 -e COMPONENT=mongodb -e ENV=dev"
            }
        }
    }
}