def branch = "main"
def repo = "git@github.com:mahesajf/retail-store-sample-app.git"
def cred = "ssh"
def dir = "~/retail-store-sample-app"
def server = "mahesajihanfadhlurrahman@34.125.190.184"

pipeline {
    agent any
    stages {
        stage('Pull From Repository') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${dir}
                        git remote add origin ${repo} || git remote set-url origin ${repo}
                        git pull origin ${branch}
                        exit
                        EOF
                    """
                }
            }
        }

        stage('Docker Compose UP') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ~/store/dist/docker-compose/
                        sudo su mahesa
                        docker compose up -d
                        exit
                        EOF
                    """
                }
            }
        }
    }
}
