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
                    customWorkspace "${GOPATH}/src/dosec.cn/public"
                }
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'develop' || env.BRANCH_NAME == 'simu') {
                        echo 'I only execute on the master branch'
                    } else {
                        echo 'I execute elsewhere'
                    }
                }
            }

        }

        stage('Pre-build') {
            agent any
            steps {
                sh 'env'
                sh '''#!/bin/bash -x
                echo "step..0.."$PWD
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
                echo "step..2.."$PWD
                '''
            }
        }

         stage('Build image and Deploy') {
                    agent any
                    steps {
                        sh 'echo "step..3.."$PWD'
                    }
         }
    }
}
