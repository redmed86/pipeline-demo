#!/usr/bin/env groovy

node('master'){
  def branch = "${env.BRANCH_NAME}"

  echo branch

  stage('Checkout') {
    checkout scm
  }

  if(env.BRANCH_NAME == 'develop') {
    pushToCloudFoundry(
            target: 'api.run.pivotal.io',
            organization: 'pipeline-demos',
            cloudSpace: 'development',
            credentialsId: 'derek_pcf',
            manifestChoice: [manifestFile: 'config/dev/manifest.yml']
    )
  }
}
