pipeline {

    agent none
    environment {
        GOPATH = '/var/jenkins_home/go'
    }

    stages {
        stage('Switch public'){
            agent {
                node {
                    label 'master'
                    customWorkspace "${GOPATH}/src/dosec.cn/jenkins"
                }
            }
            options {
                    skipDefaultCheckout true
                }
            steps {
              sh 'env'
              sh '''#!/bin/bash -x
              echo "step..-1.."$PWD
              '''
              script {
                try {
                    checkout([
                            $class: 'GitSCM',
                            branches: [[name: $BRANCH_NAME]],
                            userRemoteConfigs: [[url: 'https://github.com/Fang-Li/jenkins.git']]
                    ])
                }
                catch (Exception e) {
                    checkout([
                            $class: 'GitSCM',
                            branches: [[name: 'develop']],
                            userRemoteConfigs: [[url: 'https://github.com/Fang-Li/jenkins.git']]
                    ])
                }
              }
            }
        }

        stage('Pre-build') {
            agent any
            options {
              skipDefaultCheckout true
            }
            steps {
                sh 'env'
                sh '''#!/bin/bash -x
                echo "step..0.."$PWD
                '''
            }
        }

        stage('Build code') {
            options {
              skipDefaultCheckout true
            }
            agent {
                docker {
                    image 'hub.dosec.cn/library/golang:1.12.5-dosec'
                    customWorkspace "${GOPATH}/src/dosec.cn/hades"
                }
            }
            steps {
                sh '''#!/bin/bash -x
                echo "step..2.."$PWD
                '''
            }
        }

         stage('Build image and Deploy') {
            agent any
            options {
              skipDefaultCheckout true
            }
            steps {
                sh 'echo "step..3.."$PWD'
            }
         }
    }
}
