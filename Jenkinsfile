node{
	stage('SCM Checkout'){
		git branch: 'wartomcat', url: 'https://github.com/suman-stha/devopsprojects.git'
	}
	stage('Compile-Package'){
		def mvnHome = tool name: 'mymaven', type: 'maven'
		sh "${mvnHome}/bin/mvn package"
	}
	stage('Deploy to Tomcat'){
		sshagent(['pem-tomcatser']) {
		sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@18.191.216.34:/opt/tomcat9/webapps/'
	}
	}
