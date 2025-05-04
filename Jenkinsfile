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
                    credentialsId: 'docker-hub-credentials',
                    passwordVariable: 'DOCKER_HUB_PASSWORD',
                    usernameVariable: 'DOCKER_HUB_USERNAME'
                )]) {
                    sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USERNAME --password-stdin"
                }
            }
        }

        // stage('frontend build image') {
        //     steps {
        //         sh "docker build -t shaikharbaaz101/easyshopfrontend:${params.DOCKER_TAG} ."
        //     }
        // }

        // stage('database migration build image') {
        //     steps {
        //         sh "docker build -t shaikharbaaz101/easyshopdbmigrate:${params.DOCKER_TAG} -f scripts/Dockerfile.migration ."
        //     }
        // }

        // stage('push frontend image') {
        //     steps {
        //         sh "docker push shaikharbaaz101/easyshopfrontend:${params.DOCKER_TAG}"
        //     }
        // }

        // stage('push database migration image') {
        //     steps {
        //         sh "docker push shaikharbaaz101/easyshopdbmigrate:${params.DOCKER_TAG}"
        //     }
        // }

        stage('updating frontend/dbmigrate manifest file with correct tag') {
            steps {     
                    withCredentials([usernamePassword(
                        credentialsId: 'git-credentials',
                        usernameVariable: 'GIT_USERNAME',
                        passwordVariable: 'GIT_PASSWORD'
                    )]) {
                    // Configure Git
                    sh """
                        git config user.name "shaikharbaaz101"
                        git config user.email "shaikharbaaz101@gmail.com"
                    """
                    
                    sh """
                        # Update main application deployment
                        sed -i "s|image: shaikharbaaz/easyshopfrontend:*|image: shaikharbaaz101/easyshopfrontend:${params.DOCKER_TAG}|g" kubernetes/08-easyshop-deployment.yaml
                        
                        # Update migration job if it exists
                        if [ -f "kubernetes/12-migration-job.yaml" ]; then
                            sed -i "s|image: shaikharbaaz/easyshopdbmigrate:*|image: shaikharbaaz101/easyshopdbmigrate:${params.DOCKER_TAG}|g" kubernetes/12-migration-job.yaml
                        fi
                        
                        # Check for changes
                        if git diff --quiet; then
                            echo "No changes to commit"
                        else
                            # Commit and push changes
                            git add kubernetes/*.yaml
                            git commit -m "Update image tags to ${params.DOCKER_TAG} [ci skip]"
                            
                            # Set up credentials for push
                            git remote set-url origin https://\${GIT_USERNAME}:\${GIT_PASSWORD}@github.com/shaikharbaaz101/tws-e-commerce-app.git
                            git push origin HEAD:master
                        fi
                    """
                }
            }
        }
    }
}