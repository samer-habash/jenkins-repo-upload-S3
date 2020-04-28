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
        def repoName = GitBranch.substring(0, slug.lastIndexOf('/'))
        def repoBranch = GitBranch.tokenize('/')[0]
        
        sh "echo ${repoName} ${repoBranch}"
        sh """
          echo 'name of the repo : \$reponame'
          if [[ `aws s3 ls | grep \$reponame`  ]]
            then 
              aws s3 sync . s3://$reponame --recursive --delete
          else
            aws s3 mb s3://$repoName
          fi
        """
        }
    }
  }
}

