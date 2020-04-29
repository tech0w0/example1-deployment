pipeline {
    agent any

    stages {
       stage('Copy artifact') {
          steps {
             copyArtifacts filter: 'example1', fingerprintArtifacts: true, projectName: 'exemple1', selector: lastSuccessful()
          }
       }
       stage('Deliver') {
          steps {
             sshagent(['toobox-vagrant-key']) {
                 sh 'scp -o StrictHostKeyChecking=no example1 vagrant@10.10.50.3:~'
             }
          }
       }
    }
 }
