pipeline {
    agent any
    tools {
        maven 'maven' 
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Push') {
            agent any
            environment {
                GROUP = readMavenPom().getGroupId()
                ARTIFACT = readMavenPom().getArtifactId()
                VERSION = readMavenPom().getVersion()
            }
            steps {
               script {
                    docker.withRegistry("http://192.168.0.152:8082",'01385c55-7871-4868-a03a-889c4dc403ec') {
                    def customimage = docker.build("${GROUP}-${ARTIFACT}:${VERSION}")
                    customimage.push()
                    }
                }
                sh "docker rmi ${GROUP}-${ARTIFACT}:${VERSION}"
                sh "docker rmi 192.168.0.152:8082/${GROUP}-${ARTIFACT}:${VERSION}"
            }
        }
    }
}
