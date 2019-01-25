node('maven') {
  triggers {
    cron('H 4/* 0 0 1-5')
  }
  stage('Build') {
    git url: "git://github.com/sarguna19/SampleWebApplication.git"
    sh "mvn package"
    stash name:"war", includes:"target/SampleWebApplication.war"
  }
 
  stage('Build Image') {
    unstash name:"war"
    sh "oc start-build sample --from-file=target/SampleWebApplication.war --follow=true --wait=true"
  }
  stage('Deploy') {
    openshiftDeploy depCfg: 'sample'
    openshiftVerifyDeployment depCfg: 'sample', replicaCount: 1, verifyReplicaCount: true
    sh "oc expose svc sample"
  }
  stage('System Test') {
    sh "curl -s -X GET http://sample-first-project.192.168.99.100.nip.io/SampleWebApplication/"
  }
}
