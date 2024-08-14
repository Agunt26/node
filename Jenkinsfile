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
                        userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/Agunt26/node']]
                    ])
                }
            }
        }
     
        stage('Node JS Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Deploying Node App to Ubuntu Server') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'EC2_SSH_KEY', variable: 'SSH_KEY')]) {
                        sh '''
                            # Copy the code including the Dockerfile to the EC2 instance
                            scp -i $SSH_KEY -o StrictHostKeyChecking=no -r . ubuntu@65.0.103.61:/home/ubuntu/node-app

                            # SSH into the EC2 instance and build the Docker image
                            ssh -i $SSH_KEY -o StrictHostKeyChecking=no ubuntu@65.0.103.61 << EOF
                                # Navigate to the application directory
                                cd /home/ubuntu/node-app

                                # Ensure Docker is installed
                                docker --version

                                # Build the Docker image using the Dockerfile
                                docker build -t node-app .

                                # Stop and remove any existing container named 'node-app'
                                docker stop node-app || true
                                docker rm node-app || true

                                # Run the new container
                                docker run -d --name node-app -p 80:3000 node-app
                            EOF
                        '''
                    }
                }
            }
        }
    }
}
