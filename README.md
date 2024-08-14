# Node.js Docker Application

## Overview

This project demonstrates how to build and deploy a Node.js application using Docker. The application is built with a Dockerfile and deployed to a Docker container running on an Ubuntu EC2 instance.

## Files

- **Dockerfile**: Defines the Docker image build process.
- **Jenkinsfile**: Jenkins pipeline configuration for CI/CD.
- **README.md**: Project overview and instructions.
- **nodejsapp.yaml**: Kubernetes configuration (if needed).
- **package.json**: Node.js application dependencies and scripts.
- **server.js**: Entry point for the Node.js application.

## Build and Deployment

1. **Clone the Repository**: Ensure that Jenkins clones the repository containing these files.
2. **Build Docker Image**: Jenkins builds the Docker image using the Dockerfile.
3. **Push Docker Image**: Jenkins pushes the Docker image to DockerHub.
4. **Deploy Application**: Jenkins deploys the Docker container on an Ubuntu EC2 instance.

## Setup

Ensure that the following are configured:

- Jenkins with access to GitHub and DockerHub.
- An Ubuntu EC2 instance with Docker installed and accessible via SSH.
