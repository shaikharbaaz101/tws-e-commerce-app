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

    stage('frontend build image') {
        steps {
            sh "docker build -t easyshopfrontend:${params.DOCKER_TAG} ."
        }
    }

    stage('database migration build image') {
        steps {
            sh "docker build -t easyshopdbmigrate:${params.DOCKER_TAG} -f scripts/Dockerfile.migration ."
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


//     stage('stop existing image') {
//         steps {
//             sh """
//                     docker ps -q | xargs -r docker stop
//             """
//         }
//     }


//     stage('run image') {
//         steps {
//             sh "docker run -d -p 8082:80 nginxjenkins:${params.DOCKER_TAG}"
//         }
//     }
}
}