pipeline {
    agent any
    environment {
        dockerRun = "docker run -p 8000:80 -d --name cloudknowledges  8875022556//ci-cd-using-docker:latest"
        dockerrm = "docker container rm -f cloudknowledges"
        dockerimagerm = "docker rmi  8875022556//ci-cd-using-docker"
    }
        

    stages {
        stage('PUll') {
            steps {
                git 'https://github.com/basant0017/ci-cd-using-docker.git'
            }
        }
        
        stage("Build") {
            steps {
                sh 'docker  build -t $JOB_NAME:v1.$BUILD_ID .'
                sh 'docker  tag $JOB_NAME:v1.$BUILD_ID sd171991/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker  tag $JOB_NAME:v1.$BUILD_ID sd171991/$JOB_NAME:latest'
                
            }
        }
        
        stage("Push") {
            steps {
                withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
    // some block
    sh 'docker login -u 8875022556 -p ${docker}'
    sh 'docker  push 8875022556/$JOB_NAME:v1.$BUILD_ID'
    sh 'docker  push 8875022556/$JOB_NAME:latest'
    sh 'docker rmi $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:latest'
}
            }
        }
        
        stage("Deployment") {
            steps {
             sshagent(['ssh-agent']) {
    // some block           
   
                 
                 sh "ssh -o StricHostKeyChecking=no almalinux@15.235.147.96  ${env.dockerRun}"
    
}
        }
        }
    }
}
