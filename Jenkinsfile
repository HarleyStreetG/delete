pipeline {
    agent any


    stages {
        stage('Login and Push'){
            steps {
                script{
                  withDockerRegistry(credentialsId: 'Docker') {
                    docker.build('harleyguy/flaskapp') .push('latest')
                    }
                }
            }
        }
        stage('AWS Commands'){
            steps {
                script {
                    // sign into AWS
                    withAWS(credentials: 'AWS_Credentials', region: 'us-east-1'){ 
                        sh 'aws sts get-caller-identity'
                    }
                }
            }
        }
    }
}