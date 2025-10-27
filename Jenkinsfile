pipeline {

    environment {
        springF="amsBack"   
        angularF="amsFront"    
    }

    agent any

    stages {
        
        // Build de l’image Spring (backend) et dépôt sur DockerHub
        stage("build and push back image") {
            steps {
                script {
                    echo "====++++executing build and push back image++++===="
                    withCredentials([usernamePassword(credentialsId: 'mmnassri', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        bat "docker build -t $USER/ams_back2025 ${springF}/"
                        bat "echo $PASS | docker login -u $USER --password-stdin"
                        bat "docker push $USER/ams_back2025"
                    }
                }
            }
            post {
                always {
                    bat "docker logout"
                }
                success {
                    echo "====++++push back image execution success++++===="
                }
                failure {
                    echo "====++++push back image execution failed++++===="
                }
            }
        }

        // Build de l’image Angular (frontend) et dépôt sur DockerHub
        stage("build and push front image") {
            steps {
                script {
                    echo "====++++executing build and push front image++++===="
                    withCredentials([usernamePassword(credentialsId: 'mmnassri', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        bat "docker build -t $USER/ams_front2025 ${angularF}/"
                        bat "echo $PASS | docker login -u $USER --password-stdin"
                        bat "docker push $USER/ams_front2025"
                    }
                }
            }
            post {
                always {
                    bat "docker logout"
                }
                success {
                    echo "====++++push front image execution success++++===="
                }
                failure {
                    echo "====++++push front image execution failed++++===="
                }
            }
        }

        // Exécution de docker-compose
        stage('run docker-compose') {
            steps {
                bat 'docker compose -f docker-compose.yml up -d'
            }
            post {
                success {
                    echo "====++++Docker-compose execution success++++===="
                }
                failure {
                    echo "====++++Docker-compose execution failed++++===="
                }
            }
        }
    }
}
