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
        // We can use directly as resource scmbelow, but we need to clone the repo inside the container first in order to copy it to S3 afterwards.
        // def repositoryUrl = scm.userRemoteConfigs[0].url
        def gitrepoURL = checkout(scm).GIT_URL
        def scmBranchName = scm.branches[0].name
        // S3 bucket names must not container underscores, spaces, uppercase letters
		    def repoName = gitrepoURL.split('/')[-1].replaceAll("\\s+", "").replaceAll("_", "").replaceAll(/\.git$/, '').toLowerCase()
        def repoBranch = scmBranchName.replaceAll("/","").replaceAll(/\*/,"").replaceAll("\\s+", "").replaceAll("_", "").toLowerCase()
        def s3repoExist = sh(returnStdout: true, script: "aws s3 ls")
        if (s3repoExist.contains(repoName)) {
          println("S3 Bucket for $repoName exists")
          sh """
            # --delete : for deleting any files that are exist in source and not in S3
            aws s3 sync . s3://${repoName}-${repoBranch} --exclude '.git/*' --exclude '.idea/*' --exclude '.gitignore' --delete
            echo "S3 bucet synchronization of repo ${repoName} is finished"
          """
        }
        else {
          println("S3 Bucket $repoName does not exist")
          sh """
            aws s3 mb s3://${repoName}-${repoBranch}
            aws s3 cp . s3://${repoName}-${repoBranch} --recursive --exclude '.git/*' --exclude '.idea/*' --exclude '.gitignore'
            echo "Finished upload the repo to S3 Bucket Name : ${repoName}-${repoBranch}"
          """
        }
      }
    }
  }
}