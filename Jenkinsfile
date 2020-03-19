properties([
  parameters([
    choice(choices: ['dev', 'hml', 'prd'],description: '', name: 'ENV'),
    string(defaultValue: 'WebApache', description: '', name: 'APP_NAME', trim: false),
    string(defaultValue: '443', description: '', name: 'APP_PORT', trim: false),
    string(defaultValue: 'https://github.com/cloudacademy/static-website-example', description: '', name: 'APP_REPO', trim: false)
  ]),
  disableConcurrentBuilds(),
  buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '1', numToKeepStr: '3'))
])

pipeline {
   agent any
    environment {
        GitBranch = "master"
        EC2_IP = sh(script: 'curl http://169.254.169.254/latest/meta-data/public-ipv4', returnStdout: true)
    }
    
   stages {
        stage('check Vars') {
            steps {
                sh "echo check first if vars are compliant"
                    
                sh "echo EC2_IP: $EC2_IP"
                sh "echo ENV: $ENV"
                sh "echo APP_NAME: $APP_NAME"
                sh "echo APP_PORT: $APP_PORT"
                sh "echo APP_REPO: $APP_REPO"
            }
        }
        
        stage('checkout git') {
            steps {
                sh "pwd"
                dir("Build_WebAMI") {
                    sh "pwd"
                    git(
                        url: 'https://github.com/KarimDevOps/CICD_TP_Build_AMI.git',
                        credentialsId: '7b7ae3af-4e0e-4ed7-a384-fea719b66b10',
                        branch: "${GitBranch}"
                    )
                    sh "ls"
                }
            }
        }
        
        stage('Build AMI') {
            steps {
                dir("Build_WebAMI") {
                    dir("Packer") {
                        sh "pwd"
                        sh "ls"

                        wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'xterm']) {
                            sh "packer build \
                                -var env=$ENV \
                                -var app_repo=$APP_REPO \
                                -var app_name=$APP_NAME \
                                -var ec2_ip=$EC2_IP \
                                -var app_port=$APP_PORT \
                                buildAMI.json"
                        }
                    }
                }
            }
        }
        
        stage('Clean Jenkins Workspace') {
            steps {
                cleanWs()
            }
        }
    }
}
