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
             if ($TARGET_ENV == 'staging') {
             	sh 'docker run -t postman/newman:latest run "https://www.getpostman.com/collections/2f072fca0456a53ff5fd"'
             } else {
		sh 'docker run -t postman/newman:latest run "https://www.getpostman.com/collections/6ca22ce4c81343c17fb7"'
             }
          }
       }
    }
 }
