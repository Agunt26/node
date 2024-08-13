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
  
        stage('Build Node JS Docker Image') {
            steps {
                script {
                    sh 'docker build -t agunt2/node-app-1.0 .'
                }
            }
        }

        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'agunt2docker', variable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u agunt2 -p $DOCKER_PASSWORD'
                    }
                    sh 'docker push agunt2/node-app-1.0'
                }
            }   
        }
         
        stage('Deploying Node App to Kubernetes') {
            steps {
                script {
                    sh 'aws eks update-kubeconfig --name demo-ekscluster --region ap-south-1'
                    sh 'kubectl get ns'
                    sh 'kubectl apply -f nodejsapp.yaml'
                }
            }
        }
    }
}
