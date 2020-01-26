pipeline {
    agent any
   environment {
       docker_host = "10.1.1.200"
   }
    stages {

        stage('Cloning GITHUB Repo') {
          steps {
            sh 'rm -rf dockertest'
            sh 'git clone https://github.com/prabhu9652/dockertest.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline2/dockertest'
            sh 'cp /var/lib/jenkins/workspace/pipeline2/dockertest/*  /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t prabhu9652/pipelinetestprod:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push prabhu9652/pipelinetestprod:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://$docker_host:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://$docker_host:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 prabhu9652/pipelinetestprod:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://$docker_host:8000'
          }
        }

    }
