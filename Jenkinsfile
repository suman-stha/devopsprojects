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
		sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@52.15.194.138:/opt/tomcat9/webapps/'
	}
	}
		stage('Build Docker Image'){
        sh 'docker build -t sumand123/devopsprojects:2.0.0 .'
        
    	}
	stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]){
            sh "docker login -u care2manoj -p ${dockerHubPwd}"
    	}
        sh 'docker push sumand123/devopsprojects:2.0.0'
    	}
	stage('Run Container on Prod Server'){
        def dockerRun = 'sudo docker run -p 5000:8080 -d sumand123/devopsprojects:2.0.0'
        sshagent(['pem-tomcatser']) {
            sh "ssh -o StrictHostKeyChecking=no ec2-user@52.15.194.138 ${dockerRun}"
	}
	}

}
