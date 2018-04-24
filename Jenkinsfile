#!/usr/bin/env groovy

node('master'){
  // def props = readProperties  file: 'project.properties'
  // def version = props.version
  def branch = "${env.BRANCH_NAME}"

  echo branch
  // echo version

  stage('Checkout') {
    checkout scm
  }

  stage('Build') {
    sh 'npm install'
  }

  if(env.BRANCH_NAME == 'develop') {
    stage('Deploy to Dev') {
      withCredentials([usernamePassword(credentialsId: 'derek_pcf', passwordVariable: 'CF_PW', usernameVariable: 'CF_USER')]) {
        sh 'cf login -a api.run.pivotal.io -u $CF_USER -p $CF_PW -o pipeline_demos -s development'
        sh 'cf push -f config/dev/manifest.yml'
      }
    }

    stage('E2E Tests') {
      sh 'npm run protractor'
    }
  }

  if(env.BRANCH_NAME.startsWith('release/')) {
    stage('Deploy to Prod') {
      sh 'cf push -f config/prod/manifest.yml'
    }
  }
}
