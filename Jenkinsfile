#!/usr/bin/env groovy

node('master'){
  def branch = "${env.BRANCH_NAME}"

  echo branch

  stage('Checkout') {
    checkout scm
  }

  if(env.BRANCH_NAME == 'develop') {
    pushToCloudFoundry(
            target: 'api.local.pcfdev.io',
            organization: 'pipeline-demos',
            cloudSpace: 'development',
            credentialsId: 'derek_sauce',
            manifestChoice: [manifestFile: 'config/dev/manifest.yml']
    )
  }
}
