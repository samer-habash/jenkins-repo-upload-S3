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
        def repoUrl = checkout(scm).GIT_URL
        def repoBranch = checkout(scm).GIT_BRANCH
        sh """
          echo 'Git Branch : ${repoBranch}' 
          repoName=`echo ${repoUrl} | sed -E 's|.*/(.*)(.git)|\1|'`
          echo 'name of the repo : \$repoName'
          if [[ `aws s3 ls | grep \$repoName`  ]]
            then 
              aws s3 sync . s3://$repoName --recursive --delete
          else
            aws s3 mb s3://$repoName
          fi
        """
        }
    }
  }
}

