#!/usr/bin/env groovy

node('master'){
  def branch = "${env.BRANCH_NAME}"

  echo branch

  stage('Checkout') {
    checkout scm
  }
}
