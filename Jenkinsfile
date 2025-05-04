pipeline {
    agent any

    stages {
        stage('Checkout git repo') {
            steps {
                git branch: 'master',
                    // credentialsId: 'arbaazgit',
                    url: 'https://github.com/shaikharbaaz101/tws-e-commerce-app.git'

                sh "ls -lat"
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-credentials',  // Replace with your Jenkins credential ID
                    passwordVariable: 'DOCKER_HUB_PASSWORD',
                    usernameVariable: 'DOCKER_HUB_USERNAME'
                )]) {
                    sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USERNAME --password-stdin"
                }
            }
        }

        stage('frontend build image') {
            steps {
                sh "docker build -t shaikharbaaz101/easyshopfrontend:${params.DOCKER_TAG} ."
            }
        }

        stage('database migration build image') {
            steps {
                sh "docker build -t shaikharbaaz101/easyshopdbmigrate:${params.DOCKER_TAG} -f scripts/Dockerfile.migration ."
            }
        }

        stage('push frontend image') {
            steps {
                sh "docker push shaikharbaaz101/easyshopfrontend:${params.DOCKER_TAG}"
            }
        }

        stage('push database migration image') {
            steps {
                sh "docker push shaikharbaaz101/easyshopdbmigrate:${params.DOCKER_TAG}"
            }
        }
    }
}