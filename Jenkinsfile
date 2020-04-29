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
        def repoName = scm.getUserRemoteConfigs()[0].getUrl().lastIndexOf('/').replaceAll("\\s+", "").replaceAll("_", "").replaceAll(/\.git$/, '').toLowerCase()
        //def GitBranch = checkout(scm).GIT_BRANCH
        // def scmpath = GIT_BRANCH
        // def repoOwner = scmpath.split('/')[0]
        //def repoName = scmpath.lastIndexOf('/').replaceAll("\\s+", "").replaceAll("_", "").replaceAll(/\.git$/, '').toLowerCase()
        //def repoNameLower = repoName.substring(0, repoName.toLowerCase())
        echo $repoName
        // echo $repoOwner
        //echo $repoNameLower
        // S3 bucket cannot contain : spaces, underscores, uppercase letters
        // def repoName = GitBranch.substring(0, GitBranch.lastIndexOf('/')).toLowerCase().replaceAll("\\s+", "").replaceAll("_", "")
        // def repoBranch = GitBranch.tokenize('/')[1].toLowerCase().replaceAll("\\s+", "").replaceAll("_", "")
        // echo $repoName $repoBranch
        // def S3Check = sh(returnStdout: true, script: "aws s3 ls")
        // if (S3Check.contains(repoName)) {
        //   println("S3 Bucket for $repoName exists")
        // }
        // else {
        //   println("S3 Bucket $repoName does not exist")
        // }
        // if (check_s3) {
        //   println "S3 Bucket exists, synchronization step activated"
        //   sh """
        //       # --delete : for deleting any files that are exist in source and not in S3
        //       aws s3 sync . s3://${repoName}-${repoBranch} --exclude '.git/*' --delete
        //       echo "S3 bucet synchronization of repo ${repoName} is finished"
        //   """
        // }
        // else {
        //   println "S3 Bucket does not exist, creation step activated"
        //   sh """
        //     aws s3 mb s3://${repoName}-${repoBranch}
        //     aws s3 cp . s3://${repoName}-${repoBranch} --recursive --exclude '.git/*'
        //     echo "Finished upload the repo to S3 Bucket Name : ${repoName}-${repoBranch}"
        //   """
        // }
      }
    }
  }
}