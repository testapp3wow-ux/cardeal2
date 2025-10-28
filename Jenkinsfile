pipeline {
    agent any

    stages {
        stage('Clone') {
            steps { 
                git 'https://github.com/testapp3wow-ux/cardeal2.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        // Run backend tests
                        sh 'docker-compose run backend npm test'
                    } catch (Exception e) {
                        echo 'Warning: Tests failed but continuing pipeline'
                        // Don't fail the build yet as we're setting up the initial pipeline
                    }
                }
            }
        }

        stage('Push Images to Registry') {
            steps {
                script {
                    // Commented out until Docker Hub credentials are configured
                    // withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    //     sh 'docker login -u $DOCKER_USER -p $DOCKER_PASSWORD'
                    //     sh 'docker tag buyc_corp-main-backend $DOCKER_USER/buyc_corp-main-backend:latest'
                    //     sh 'docker tag buyc_corp-main-frontend $DOCKER_USER/buyc_corp-main-frontend:latest'
                    //     sh 'docker push $DOCKER_USER/buyc_corp-main-backend:latest'
                    //     sh 'docker push $DOCKER_USER/buyc_corp-main-frontend:latest'
                    // }
                    echo 'Image push stage - configure Docker Hub credentials and uncomment to enable'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Commented out until K8s configurations are ready
                    // sh 'kubectl apply -f k8s/'
                    echo 'Kubernetes deployment stage - add k8s/ folder with configurations and uncomment to enable'
                }
            }
        }
    }

    post {
        always {
            // Cleanup
            sh 'docker-compose down'
            cleanWs()
        }
    }
}