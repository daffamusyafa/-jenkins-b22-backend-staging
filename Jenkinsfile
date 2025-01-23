def secret = 'ssh-global'
def server = 'team1@103.127.139.123'
def directory = 'team1-docker'
def branch = 'main'
def image = 'daffamusyafa/wayshub-be-b22:latest'

pipeline {
    agent any
    stages {
        stage ('pulling new code'){
            steps{
                sshagent([secret]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}/backend
                    git pull origin ${branch}
                    exit
                    EOF"""
                }
            }
        }
        stage ('Build Process'){
            steps{
                sshagent([secret]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}/backend
                    docker build --no-cache -t ${image} .
                    exit
                    EOF"""
                }
            }
        }    
        stage ('Deploy'){
            steps{
                sshagent([secret]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}/backend
                    docker compose down
                    docker compose up -d
                    exit
                    EOF"""
                }
            }
        }
    }
}
