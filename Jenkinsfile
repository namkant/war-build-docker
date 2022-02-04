node {

   def registryProjet='registry.gitlab.com/namkant/wartest'
   def IMAGE="${registryProjet}:version-${env.BUILD_ID}"

    stage('Clone') {
        git https://github.com/namkant/war-build-docker.git
    }
    stage('Maven package') {
        sh 'mvn package'
    }
    def img = stage('Build') {
          docker.build("$IMAGE",  '.')
    }
    stage('Run') {
          img.withRun("--name run-$BUILD_ID -p 8081:8080") { c ->
            sh 'docker ps'
            sh 'netstat -ntaup'
            sh 'sleep 10s'
            sh 'curl 192.168.126.128:8081'
          }
    }

    stage('Push') {
          docker.withRegistry('https://registry.gitlab.com', 'reg1') {	
               img.push 'latest'
	       img.push()
          }
    }

}
