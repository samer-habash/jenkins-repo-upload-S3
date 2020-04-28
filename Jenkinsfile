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
        // S3 bucket cannot contain : spaces, underscores, uppercase letters
        def repoName = GitBranch.substring(0, GitBranch.lastIndexOf('/')).toLowerCase().replaceAll("\\s+", "").replaceAll("_", "")
        def repoBranch = GitBranch.tokenize('/')[1].toLowerCase().replaceAll("\\s+", "").replaceAll("_", "")
        sh label: 'Checking if S3 exists', script: '''
            #!/usr/bin/env bash
            aws s3 ls
            grep ${repoName}
            aws s3 ls | grep ${repoName} || true
            status=("${PIPESTATUS[@]}")
            AWSCODE=${status[0]}
            GREPCODE=${status[1]}
            echo $AWSCODE $GREPODE
            if [ $GREPCODE -eq 0 ]
            then
                # --delete : for deleting any files that are exist in source and not in S3
                aws s3 sync . s3://${repoName}-${repoBranch} --exclude '.git/*' --delete
                echo "Finished creation of the repo to S3 Bucket Name : ${repoName}-${repoBranch}"
            else
                aws s3 mb s3://${repoName}-${repoBranch}
                aws s3 cp . s3://${repoName}-${repoBranch} --recursive --exclude '.git/*'
                echo "Finished upload the repo to S3 Bucket Name : ${repoName}-${repoBranch}"
            fi
        '''
        }
      }
    }
  }
}