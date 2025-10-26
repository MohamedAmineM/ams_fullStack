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
                    withCredentials([usernamePassword(credentialsId: 'fullstack_dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh "docker build -t $USER/ams_back ${springF}/"
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push $USER/ams_back"
                    }
                }
            }
            post {
                always {
                    sh "docker logout"
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
                    withCredentials([usernamePassword(credentialsId: 'fullstack_dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh "docker build -t $USER/ams_front ${angularF}/"
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push $USER/ams_front"
                    }
                }
            }
            post {
                always {
                    sh "docker logout"
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
                sh 'docker compose -f docker-compose.yml up -d'
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
