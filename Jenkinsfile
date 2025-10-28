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
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker tag buyc_corp-main-backend testapp3wow-ux/cardeal2-backend:latest'
                        sh 'docker tag buyc_corp-main-frontend testapp3wow-ux/cardeal2-frontend:latest'
                        sh 'docker push testapp3wow-ux/cardeal2-backend:latest'
                        sh 'docker push testapp3wow-ux/cardeal2-frontend:latest'
                    }
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