pipeline {
    agent any
    
    tools {
        nodejs 'node' // Ensure this matches the name configured in Global Tool Configuration
    }
    
    stages {
        stage("Clone code from GitHub") {
            steps {
                script {
                    checkout([$class: 'GitSCM', 
                        branches: [[name: '*/main']], 
                        userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/your-repo/node']]
                    ])
                }
            }
        }
     
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t node-app .'
                }
            }
        }

        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'DOCKERHUB_CREDENTIALS', variable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u your-dockerhub-username -p $DOCKER_PASSWORD'
                    }
                    sh 'docker tag node-app your-dockerhub-username/node-app:latest'
                    sh 'docker push your-dockerhub-username/node-app:latest'
                }
            }
        }
         
        stage('Deploying Node App to Ubuntu Server') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'EC2_SSH_KEY', variable: 'SSH_KEY')]) {
                        sh '''
                            ssh -i $SSH_KEY -o StrictHostKeyChecking=no ubuntu@<EC2_PUBLIC_IP> << EOF
                                # Stop and remove any existing container named 'node-app'
                                docker stop node-app || true
                                docker rm node-app || true

                                # Pull the latest Docker image from DockerHub
                                docker pull your-dockerhub-username/node-app:latest

                                # Run the new container
                                docker run -d --name node-app -p 80:3000 your-dockerhub-username/node-app:latest
                            EOF
                        '''
                    }
                }
            }
        }
    }
}
