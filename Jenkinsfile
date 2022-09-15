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
                docker build -t carmanagementdashboard/first-trial:v1 .
                '''
            }
        }
        stage('Push Image') {
            steps {
                sh '''
                set +x
                docker login --username=sulistiowatiayu --password=$dockerPassword
                set -x
                docker push sulistiowatiayu/first-trial:v1
                '''
            }
        }
        stage('Deploy image') {
            steps {
                sh '''
                docker run -it --rm --name carmanagementdashboard first-trial:v1 
                docker run -p 3000:3000 -d --name carmanagementdashboard first-trial:v1 
                '''
            }
        }
    }
}
