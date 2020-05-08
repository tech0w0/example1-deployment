pipeline {
    agent any

    parameters {
       //choice(name: 'TARGET_ENV', choices: ['staging', 'production'], description: 'Please choose an environment')
       gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH'
    }

    stages {
       stage('Copy artifact') {
          steps {
             copyArtifacts filter: 'example1', fingerprintArtifacts: true, projectName: 'example1', selector: lastSuccessful()
          }
       }
       stage('Deliver Production') {
          when {
              expression { params.BRANCH == 'master'}
          }
          steps {
                 ansiblePlaybook credentialsId: 'toobox-vagrant-key',
                 inventory: "inventories/production/hosts.ini",
                 playbook: 'playbook.yml',
                 disableHostKeyChecking: true
          }
       }
       stage('Deliver Staging'){
          when {
             expression { params.BRANCH == 'develop' }
          } 
          steps {
                 ansiblePlaybook credentialsId: 'toobox-vagrant-key',
                 inventory: "inventories/staging/hosts.ini",
                 playbook: 'playbook.yml',
                 disableHostKeyChecking: true
          }
       }             
       stage('Integration Test') {
          steps {
             sh 'docker run -t postman/newman:latest run "https://www.getpostman.com/collections/2f072fca0456a53ff5fd"'
          }
       }
       stage('Job description') {
           when {
             expression { params.BRANCH == 'develop' }
          } 
          steps {
             script {
                currentBuild.rawBuild.project.description = 'Staging'
             }
          }
       }
         stage('Job description2') {
           when {
             expression { params.BRANCH == 'master' }
          } 
          steps {
             script {
                currentBuild.rawBuild.project.description = 'Prodution'
             }
          }
       }
    }
 }
