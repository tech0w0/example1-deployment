pipeline {
    agent any

    stages {
       stage('Copy artifact') {
          steps {
             copyArtifacts filter: 'example1', fingerprintArtifacts: true, projectName: 'example1', selector: lastSuccessful()
          }
       }
       stage('Deliver') {
          steps {
                 ansiblePlaybook credentialsId: 'toobox-vagrant-key', inventory: 'hosts.ini', playbook: 'playbook.yml'
          }
       }
       stage('Integration Test') {
          agent {
             docker {
                image 'postman/newman'
                args '--entrypoint='
             }
          }
          steps {
             sh 'newman run "https://www.getpostman.com/collections/2f072fca0456a53ff5fd"'
          }
       }
    }
 }
