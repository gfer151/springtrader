pipeline {
  agent any
  stages {

    // Note: Add build stage here
    
  stage('Build') {
  agent {
    label "lead-toolchain-skaffold"
  }
  steps {
    container('skaffold') {
      sh "skaffold build --file-output=image.json"
      stash includes: 'image.json', name: 'build'
      sh "rm image.json"
    }
  }
}


    // Note: Add deploy stage here
    
    version: 0.2
phases:
  pre_build:
    commands:
      - aws eks update-kubeconfig --name $CLUSTER
  build:
    commands:
      - skaffold deploy -a image.json -n $STAGING_NAMESPACE


    // Note: Add gating stage here
    
    stage ('Manual Ready Check') {
  agent none
  when {
    branch 'master'
  }
  options {
    timeout(time: 30, unit: 'MINUTES')
  }
  input {
    message 'Deploy to Production?'
  }
  steps {
    echo "Deploying"
  }
}


    // Note: Add prod stage here

version: 0.2
phases:
  pre_build:
    commands:
      - aws eks update-kubeconfig --name $CLUSTER
  build:
    commands:
      - skaffold deploy -a image.json -n $PROD_NAMESPACE


  }
}
