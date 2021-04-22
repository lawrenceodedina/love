pipeline {
    agent any
    tools {
  maven 'M2_HOME'
}
    triggers {
  pollSCM '* * * * *'
}
    stages {
        stage('Build war file') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                sh 'mvn package'
            }
        }
        stage('Testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy docker image') {
            steps {
                script{
                    checkout scm
                    docker.withRegistry('', 'dockerid'){
                        def app=docker.build("femiodedina/love:${env.BUILD_NUMBER}")
                        app.push()
                        app.push("latest")
                    }
                }
            }
        }
    }
}