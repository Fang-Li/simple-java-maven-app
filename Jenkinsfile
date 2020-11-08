pipeline {
    agent none
    environment {
        GOPATH = '/home/lif/repo/dev'
    }

    stages {

        stage('Pre-build') {
            agent any
            steps {
                sh '''
                '''
            }
        }

        stage('Build code') {
            agent {
                docker {
                    image 'hub.dosec.cn/library/golang:1.12.5-dosec'
                    customWorkspace "${GOPATH}/src/dosec.cn/hades"
                }
            }
            steps {
                sh '''#!/bin/bash -x
                if [[ $BRANCH_NAME == "develop" ]];then
                    make build-dev
                else
                    make build-release
                fi
                '''
            }
        }
    }
}
