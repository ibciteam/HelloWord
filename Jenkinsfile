pipeline {
enviornment {
  registry="hub.docker.com/adua/test"
  registryCredential = 'dockerhub-cicd-ashish' //Created in Jenkins with ID
  }
agent {dockerfile true }

stages
{
stage('Build Image')
{
steps{
def testImage = docker.build registry + ":v1"
}
}
stage('Deploying Image to Docker Registry')
{
steps{
script{
docker.withRegistry(registry,registryCredential){
testImage.push()
}
}
}
stage('Deploy Image to K8 cluster')
{
steps{
script{
kubectl apply -f jenkins-deployment.yaml
kubectl apply -f jenkins-service.yaml
}
}
}
stage('Upload Artifact to s3 Bucket')
{
steps{
dir('/Path to jenkins job artifact or path to Project workspace')
{

withAWS(region:'S3 Region to where artifacts need to be uploaded',credentials:'AWS Credentials created where EC2 instance for Jenkins server resides')
{
s3Upload(bucket:"yourBucketName", workingDir:'dist', includePathPattern:'**/*');
}
}
}
}
}
}