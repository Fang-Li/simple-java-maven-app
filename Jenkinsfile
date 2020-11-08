pipeline {
    agent none
    environment {
        GOPATH = '/var/jenkins_home/go'
    }

    stages {

        stage('Pre-build') {
            agent any
            steps {
                sh '''
                echo "step..0.."$PWD
                '''
            }
        }

        stage('Build code') {
            steps {
                sh '''#!/bin/bash -x
                echo "step..1.."$PWD
                '''
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
    }
}
