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
           environment {
                BRANCH = 'develop'
                COMMIT = ""
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
                //echo "branch_name="$BRANCH_NAME
                //echo "branch_name2="env.BRANCH_NAME
                echo "$env"

                try {
                    echo 'Pulling...' + env.BRANCH_NAME
                    echo env.BRANCH env.COMMIT
                    BC=(env.COMMIT ? env.COMMIT : env.BRANCH)
                    echo $BC
                    checkout([
                            $class: 'GitSCM',
                            branches: [[name: (env.COMMIT ? env.COMMIT : env.BRANCH )]],
                            extensions: [[$class: 'LocalBranch'],[$class: 'PruneStaleBranch']],
                            userRemoteConfigs: [[url: 'https://github.com/Fang-Li/jenkins.git']]
                    ])
                }
                catch (Exception e) {
                    echo e
                    echo 'Pulling...' + "develop"
                    checkout([
                            $class: 'GitSCM',
                            branches: [[name: 'develop']],
                            extensions: [[$class: 'LocalBranch'],[$class: 'PruneStaleBranch']],
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
