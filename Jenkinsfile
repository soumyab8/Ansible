pipeline {
    agent any 

parameters {
         choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Chose the environment')
         string(name: 'COMPONENT', defaultValue: '', description: 'Enter the name of the component')
    }
environment { 
        SSH_CRED = credentials('SSH-Centos7')
        GIT = credentials('github-token')
}
    stages{

        stage('Lint Checks') {  
            when { branch pattern: "feature-.*", comparator: "REGEXP"} // This will be executed against the feature branch only
            steps {
                sh "env"
                sh "echo Style Checks"
                sh "echo running is feature branch"
            }
        }

        stage('Do a dry-run') {
            when { branch pattern: "PR-.*", comparator: "REGEXP"}        // This will be executed only when you raise a PR
            steps {
                sh "env"   // Just to see the environment variables as a part of the pipeline
                sh "ansible-playbook robo-dryrun.yml -e ansible_user=${SSH_CRED_USR} -e ansible_password=${SSH_CRED_PSW} -e COMPONENT=${params.COMPONENT} -e ENV=${params.ENV}"
            }
        }

        stage('Tagging') {
            when { branch 'main' }       
            steps {
                git branch: 'main', url: "https://${GIT_USR}:${GIT_PSW}@github.com/soumyab8/Ansible.git"   // Git Clone
                sh "bash -x auto-tag.sh"   
            }
        }

        // stage('promote to prod') {
        //     when { branch 'main' }
        //     steps{
        //         sh "echo runs only when you push a tag"                
        //     }
        // }
    }
}


// Tag will be pushed only against the main branch and when we push a tag that means new version ready and that means some stage has to run
// Scan should happen automatic

// Release is like marking a version 
// Every release doesn't have to hit prod,  one versions of your choice which gives nice feature will only be marked as Prod-Release