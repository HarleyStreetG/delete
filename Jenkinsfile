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
                    withAWS(credentials: 'AWSCredentials', region: 'us-east-1'){ 
                        sh 'aws sts get-caller-identity'
                    }
                }
            }
        }
        stage('Kubernetes login'){
            steps{
                script{
                    withAWS(credentials: 'AWSCredentials', region: 'us-east-1'){
                        sh 'aws eks update-kubeconfig --region us-east-1 --name VETTEC'
                    }
                }
            }
        }
        stage('Create Namespace'){
            steps {
                script {
                    withAWS(credentials: 'AWSCredentials', region: 'us-east-1') {
                        try {
                            sh 'kubectl apply -f manifest.yaml'
                            sh 'kubectl rollout restat deployment flask-deployment'
                        } catch (Exception e) {
                            echo 'Exception occured: ' + e.toString()
                            echo 'Handled the Exception!'
                        }
                    }
                }
            }
        }
    }
}