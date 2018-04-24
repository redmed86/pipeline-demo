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

  stage('Unit Tests') {
    sauce('derek_sauce') {
      withCredentials([usernamePassword(credentialsId: 'derek_sauce_key', passwordVariable: 'SAUCE_ACCESS_KEY', usernameVariable: 'SAUCE_USERNAME')]) {
        sauceconnect(options: '-u $SAUCE_USERNAME -k $SAUCE_ACCESS_KEY -i derek_test_tunnel', sauceConnectPath: '') {
          sh 'npm run test-single-run'
        }
      }
    }
  }

//  stage('Unit Tests') {
//    sh 'npm run test-single-run'
//  }

  if(env.BRANCH_NAME == 'develop') {
    stage('Deploy to Dev') {
      sh 'cf push -f config/dev/manifest.yml'
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
