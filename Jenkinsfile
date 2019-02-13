node('maven') {
  stage('Build') {
    git url: "git://github.com/sarguna19/SampleWebApplication.git"
    sh "mvn package"
    stash name:"war", includes:"target/SampleWebApplication.war"
  }
 
  stage('Build Image') {
    unstash name:"war"
    sh "oc start-build webapp --from-file=target/SampleWebApplication.war --follow=true --wait=true"
  }
  stage('Deploy') {
    openshiftDeploy depCfg: 'webapp'
    openshiftVerifyDeployment depCfg: 'webapp', replicaCount: 1, verifyReplicaCount: true
  }
  stage('System Test') {
    sh "curl -s -X GET http://webapp-twg-demo.192.168.99.100.nip.io/SampleWebApplication/"
  }
}
