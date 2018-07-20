pipeline {
  agent {
    node {
      label 'DOCKER'
    }

  }
  stages {
    stage('Poll SCM') {
      steps {
        git(url: 'https://github.com/cit-astrum/jenkins-docker.git', branch: 'master')
      }
    }
    stage('Docker Build') {
      steps {
        sh '''cd nhttpd
IMAGE=$(cat Dockerfile | head -1 |sed -e "s/#//")
docker build -t $IMAGE .'''
      }
    }
    stage('Docker Publish') {
      steps {
        sh '''cd nhttpd
IMAGE=$(cat Dockerfile | head -1 |sed -e \'s/#//\')
docker push $IMAGE'''
      }
    }
    stage('Update Cluster') {
      steps {
        sh '''cd nhttpd
IMAGE=$(cat Dockerfile | head -1 |sed -e \'s/#//\')
kubectl set image deployment/student nhttpd=$IMAGE'''
      }
    }
  }
  environment {
    NAME = 'DEVOPS'
  }
}