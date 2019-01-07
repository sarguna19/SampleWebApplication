node('maven') {
  stage('Build') {
    git url: "git://github.com/sarguna19/SampleWebApplication.git"
    sh "mvn package"
    stash name:"war", includes:"target/SampleWebApplication.war"
  }
 
  stage('Build Image') {
    unstash name:"war"
    sh "oc start-build sample-service-pipeline --from-file=target/SampleWebApplication.war -n my-new-project"
  }
  stage('Deploy') {
    openshiftDeploy depCfg: 'sample'
    openshiftVerifyDeployment depCfg: 'sample', replicaCount: 1, verifyReplicaCount: true
  }
  stage('System Test') {
    sh "curl -s -X POST http://sample:8080/SampleWebApplication/"
  }
}
