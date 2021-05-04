pipeline {
    environment {
        registry = "naz513/nahidrepo"
        registryCredentail = 'docker_id'
        dockerImage = ''
    }
    agent {
        node {
            label 'nodejs'
        }
    }
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages {
        stage('Cloning our Git') {
            steps {
                git url: 'https://github.com/Naz513/test2.git', branch: 'main'
            }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredentail) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
