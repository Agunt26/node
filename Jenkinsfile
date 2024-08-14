pipeline {
    agent any
    
    tools {
        nodejs 'nodejs' // Ensure this matches the name configured in Global Tool Configuration
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
                        sh 'docker login -u agunt2 -p Abhi@1234'
                    }
                    sh 'docker tag node-app agunt2/node-app:latest'
                    sh 'docker push agunt2/node-app:latest'
                }
            }
        }
         
        stage('Deploy Node App to Ubuntu Server') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'EC2_SSH_KEY', variable: 'SSH_KEY')]) {
                        sh '''
                            ssh -i $SSH_KEY -o StrictHostKeyChecking=no ubuntu@65.0.103.61 << EOF
                                # Stop and remove any existing container named 'node-app'
                                docker stop node-app || true
                                docker rm node-app || true

                                # Pull the latest Docker image from DockerHub
                                docker pull agunt2/node-app:latest

                                # Run the new container
                                docker run -d --name node-app -p 80:3000 agut2node-app:latest
                            EOF
                        '''
                    }
                }
            }
        }
    }
}
