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
                 ansiblePlaybook credentialsId: 'toobox-vagrant-key', inventory: 'inventories/staging/hosts.ini', playbook: 'playbook.yml'
          }
       }
       stage('Integration Test') {
          steps {
             sh 'docker run -t postman/newman:latest run "https://www.getpostman.com/collections/2f072fca0456a53ff5fd"'

          }
       }
    }
 }
