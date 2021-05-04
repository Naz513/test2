pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/Naz513/test2', branch: 'main'
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "https://msitest.jfrog.io/artifactory",
                    credentialsId: 'jfrog_id'
                )
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    def app = docker.build("msitest.jfrog.io/msaquib/my:1")
                }
            }
        }
        stage('Push Image to Artifactory') {
            steps {
                rtDockerPush(
                    serverId: "ARTIFACTORY_SERVER",
                    image: "msitest.jfrog.io/msaquib/my:1",
                    targetRepo: 'msaquib'
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}
