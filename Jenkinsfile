peline {
    agent any

    stages {
       stage('Copy artifact') {
          steps {
             copyArtifacts filter: 'example1', fingerprintArtifacts: true, projectName: 'example1', selector: lastSuccessful()
          }
       }
       stage('Deliver') {
          steps {
             sshagent(['toobox-vagrant-key']) {
                 sh 'scp example1 vagrant@10.10.50.3:~'
             }
          }
       }
    }
 }
