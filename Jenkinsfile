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
    





















//     stage('build image') {
//         steps {
//             sh "docker build -t nginxjenkins:${params.DOCKER_TAG} ."
//         }
//     }

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