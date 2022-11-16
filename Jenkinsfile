node(){
stage("git"){
checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/naveenandukuri/mavenproj.git']]])
}
stage("maven"){
sh 'mvn package'
}
stage("sonar"){
sh '''mvn sonar:sonar \
  -Dsonar.projectKey=mavenproj \
  -Dsonar.host.url=http://3.145.203.146:9000 \
  -Dsonar.login=54766543c745cefc53889e75bee70acfc6b81908'''
}
stage("nexus"){
nexusArtifactUploader artifacts: [[artifactId: '$BUILD_TIMESTAMP', classifier: '', file: 'webapp/target/webapp.war', type: 'war']], credentialsId: 'NEXUS-CRED', groupId: 'prod', nexusUrl: '3.143.234.50:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'test', version: '$BUILD_ID'
}
stage("tomcat"){
deploy adapters: [tomcat9(credentialsId: 'TOMCAT-CRED', path: '', url: 'http://18.222.223.30:8080/')], contextPath: 'webapp', onFailure: false, war: '**/*.war'
}
}




