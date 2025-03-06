pipeline {
   agent any

 environment{
        AWS_REGION = 'us-east-1'
        IMAGE_ECR_REPO  = '288761740567.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci'
        ECR_REPO = 288761740567.dkr.ecr.us-east-1.amazonaws.com
 }

     Stages{
        stage('CodeScan'){
        steps{
            sh 'trivy fs .  -o result.html'
            sh 'cat result.html'
           
        }
    }
    stage('dockerLogin'){
        steps{
            sh 'aws ecr get-login-password --region $AWS_REGION | \
            docker login --username AWS \
            --password-stdin 288761740567.dkr.ecr.us-east-1.amazonaws.com'
        }
    }
    stage('dockerImageBuild'){
        steps{
            sh 'docker build -t jenkins-ci .'
            sh 'docker build -t imageversion . '
        }
}
    stage('dockerImageTag'){
        steps{
            sh 'docker tag jenkins-ci:latest\
             288761740567.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci:latest'
            sh 'docker tag imageversion \
           $IMAGE_ECR_REPO:v1.$BUILD_NUMBER'
        }    
        }
    
    stage('pushImage'){
        steps{
            sh 'docker push \
            288761740567.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci:latest'
            sh 'docker push \
            288761740567.dkr.ecr.us-east-1.amazonaws.com/jenkins-ci:v1.$BUILD_NUMBER'
        }
    }
   } 
   }