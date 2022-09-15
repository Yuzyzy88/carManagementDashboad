pipeline {
    agent any
    stages {
        stage('SCM Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout', deleteUntrackedNestedRepositories: true]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'githuub-login', url: 'https://github.com/Yuzyzy88/carManagementDashboad.git']]])
            }
        }
        stages('Create Image') {
            steps {
                sh '''
                docker build -t carManagementDashboard/carImage:v1 .
                '''
            }
        }
        stages('Push Image') {
            steps{
                sh '''
                set +x
                docker login --username=sulistiowatiayu --password=${docker-password}
                set -x
                '''
            }
        }
    }
}
