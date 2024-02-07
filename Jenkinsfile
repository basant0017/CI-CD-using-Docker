                def dockerrun = 'docker run -p 8000:80 -itd --name compose 8875022556/ci-cd-using-docker:latest'
                def dockerrm = 'docker container rm -f compose'
               def dockerimagerm = 'docker rmi -f 8875022556/ci-cd-using-docker'
pipeline {
    agent any

    stages {
        stage('Build Dockerfile') {
            steps {
                  sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                  sh 'docker image tag $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:v1.$BUILD_ID' 
                 sh 'docker image tag $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:latest'                        
            }
        }
        stage('Push Image To Docker HUB'){
            steps{
            withCredentials([string(credentialsId: 'docker', variable: 'docker')]) {
    // some block
                sh 'docker login -u 8875022556 -p ${docker} '
                sh 'docker image push 8875022556/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image push 8875022556/$JOB_NAME:latest'
                sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:v1.$BUILD_ID 8875022556/$JOB_NAME:latest '
                  }
                }
            }
        stage('Deployment Of Docker Container'){
            steps{
                sshagent(['ssh-agent']) {
    // some block
                   sh "ssh -o StrictHostKeyChecking=no almalinux@15.235.147.96 ${dockerrm}" 
                   sh "ssh -o StrictHostKeyChecking=no almalinux@15.235.147.96 ${dockerimagerm}"
                   sh "ssh -o StrictHostKeyChecking=no almalinux@15.235.147.96 ${dockerrun}"    
}
                }
        }
      }
    }
