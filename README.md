Usage:

Upload/Sync repo to S3 bucket upon commit change .

1) Used Pull SCM for Jenkins every 2 minites, I did not use post-commit webhook since my jenkins is on my local machine.

2) Used Kubernetes-Plugin with a small size 102 MB of an alpine aws-cli v2 installed.

3) Docker Hub image : samer1984/aws-cli-alpine:v1

4) The credentials of aws can be made in two ways (I personally prefer the first one) :
  a) Create a secret yaml file that has the credentials of AWS
  b) Create a confiMap yaml file and push as a volume mount in docker image 
  c) I have checked afterwards for a secret volume instead of configMap , you can try it by yourself but it is the same as secret

5) Used a Jenkinsfile that will be clone the repo inside the contianer image aws-cli-alpine and then sync the content with the S3 Bucket (If the Bucket does not exist then it will add it and copy the repo)

6) It was a challenge of not using the AWS SDK , or even the CloudBees credentials of AWS ,but I made it directly through kubernetes.

NOTE: from my point of view I have the full functionality of aws-cli rather than learning the SDK functions and bla bla ....


