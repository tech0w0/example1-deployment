pipeline {
    agent any

    parameters {
       choice(name: 'TARGET_ENV', choices: ['staging', 'production'], description: 'Please choose an environment')
       //gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH'
    }

    stages {
       stage('Copy artifact') {
          steps {
             copyArtifacts filter: 'example1', fingerprintArtifacts: true, projectName: 'example1', selector: lastSuccessful()
          }
       }
       stage('Deliver Production') {
          steps {
                 ansiblePlaybook credentialsId: 'toobox-vagrant-key',
                 inventory: "inventories/${TARGET_ENV}/hosts.ini",
                 playbook: 'playbook.yml',
                 disableHostKeyChecking: true
          }
       }            
       stage('Integration Test') {
          steps {
             	sh 'docker run -v $HOME/workspace/example1-deployment/inventories/${TARGET_ENV}:/etc/newman -t postman/newman run "https://www.getpostman.com/collections/fc43637f9e05ae4486fb" -e postman_env.json' 
          }
       }
    }
 }
