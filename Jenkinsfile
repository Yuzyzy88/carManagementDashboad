pipeline {
    agent any
    stages {
        stage('SCM Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout', deleteUntrackedNestedRepositories: true]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github-login', url: 'https://github.com/Yuzyzy88/carManagementDashboad.git']]])
            }
        }
        stage('Create Image') {
            steps {
                sh '''
                docker image rm sulistiowatiayu/first-trial:v1 || echo "No existing image found"
                docker build --no-cache -t 10.8.60.126:5000/first-trial:v2 . 
                '''
            }
        }
        stage('Push Image') {
            steps {
                sh '''
                set +x
                docker login --username=admin --password=$nexusPassword 10.8.60.126:5000
                set -x
                docker push 10.8.60.126:5000/first-trial:v2
                '''
            }
        }
        stage('Deploy image') {
            steps {
                sh '''
                (docker stop carmanagment && docker rm carmanagment) || echo "No existing container running";
                docker run -p 3000:80 -d --name carmanagment sulistiowatiayu/first-trial:v2
                '''
            }
        }
    }
}
