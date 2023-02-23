pipeline {
    agent any

    environment{
        DOCKER = credentials('Docker')
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t harleyguy/flaskapp .'
            }
        }
        stagr('Login'){
            steps {
                echo '$DOCKER | docker login -u harleyguy --password-stdin'
            }
        }
    }
}