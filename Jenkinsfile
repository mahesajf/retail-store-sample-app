def branch = "staging"
def repo = "git@github.com:mahesajf/retail-store-sample-app.git"
def cred = "ssh"
def dir = "~/retail-store-sample-app"
def server = "mahesajihanfadhlurrahman@34.125.190.184"

pipeline {
    agent any

    stages {
        stage('Repository pull') {
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

        stage('Image build') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${dir}
                        docker build -t ${imagename}:latest .
                        exit
                        EOF
                    """
                }
            }
        }

        stage('Running the image in a container') {
            steps {
                sshagent([cred]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${dir}
                        docker container stop ${imagename} || true
                        docker container rm ${imagename} || true
                        docker run -d -p 3000:3000 --name="${imagename}"  ${imagename}:latest
                        exit
                        EOF
                    """
                }
            }
        }
    }
}
