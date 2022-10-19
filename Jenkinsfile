pipeline {
    agent any
    stages {

        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([
                            password(
                                defaultValue: 'admin',
                                description: 'User for nexus',
                                name:'nexusUser'
                            ),
                            password(
                                defaultValue: '060201&160288Ka',
                                description: 'Password for nexus',
                                name:'nexusPassword'
                            )
                        ])
                    ])
                }
            }
        }

        stage('SCM Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout', deleteUntrackedNestedRepositories: true]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github-login', url: 'https://github.com/Yuzyzy88/carManagementDashboad.git']]])
            }
        }
        stage('Create Image') {
            steps {
                sh '''
                docker image rm sulistiowatiayu/first-trial:v1 || echo "No existing image found"
                docker build --no-cache -t sulistiowatiayu/first-trial:v8 . 
                '''
            }
        }
        stage('Push Image') {
            steps {
                sh '''
                set +x
                docker login --username=nexusUser --password=$nexusPassword 
                set -x
                docker push sulistiowatiayu/first-trial:v8
                '''
            }
        }
        stage('Deploy image') {
            steps {
                sh '''
                (docker stop carmanagment && docker rm carmanagment) || echo "No existing container running";
                docker pull 10.8.60.126:5000/first-trial:v2
                docker run -p 3000:80 -d --name carmanagment 10.8.60.126:5000/first-trial:v2
                '''
            }
        }
    }
}
