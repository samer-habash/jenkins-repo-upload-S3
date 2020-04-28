podTemplate(cloud: 'kubernetes-cluster1', label: 'pod-label-cluster1',
    containers: [
		containerTemplate(
		  name: 'aws-cli-secret',
		  image: 'samer1984/aws-cli-alpine:v1',
		  ttyEnabled: true,
		  envVars: [
			envVar(key: 'AWS_DEFAULT_REGION', value: 'us-east-1'),
			secretEnvVar(key: 'AWS_ACCESS_KEY_ID', secretName: 'aws-configure', secretKey: 'AWS_ACCESS_KEY_ID'),
			secretEnvVar(key: 'AWS_SECRET_ACCESS_KEY', secretName: 'aws-configure', secretKey: 'AWS_SECRET_ACCESS_KEY')]
		)
	]
)
{
  node('pod-label-cluster1') {
    stage('isuuing aws commands') {
      container('aws-cli-secret') {
        def GitBranch = checkout(scm).GIT_BRANCH
        def repoName = GitBranch.substring(0, GitBranch.lastIndexOf('/')).toLowerCase()
        def repoBranch = GitBranch.tokenize('/')[1].toLowerCase()
        
        sh """
            if [[ `aws s3 ls | grep ${repoName}`  ]]
            then
              aws s3 sync . s3://${repoName}-${repoBranch} --recursive --delete
              echo "Finished creation of the repo to S3 Bucket Name : ${repoName}-${repoBranch}"
            else
              aws s3 mb s3://${repoName}-${repoBranch}
              aws s3 cp . s3://${repoName}-${repoBranch} --recursive
              echo "Finished upload the repo to S3 Bucket Name : ${repoName}-${repoBranch}"
            fi
        """
        }
    }
  }
}

