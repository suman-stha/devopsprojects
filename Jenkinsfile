node{
	stage('SCM Checkout'){
		git branch: 'wartomcat', url: 'https://github.com/suman-stha/devopsprojects.git'
	}
	stage('Compile-Package'){
		def mvnHome = tool name: 'mymaven', type: 'maven'
		sh "${mvnHome}/bin/mvn package"
	}
	 stage('Build Docker Image'){
        sh 'docker build -t sumand123/maven-project:7.0.0 .'
    }
	stage('Deploy to Tomcat'){
		sshagent(['tomcat']) {
		sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@18.206.251.244:/opt/tomcat9/webapps/'
	}
	}
		stage('Build Docker Image'){
        sh 'docker build -t sumand123/devopsprojects:2.0.0 .'
        
    	}
	stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]){
            sh "docker login -u sumand123 -p ${dockerHubPwd}"
    	}
        sh 'docker push sumand123/devopsprojects:2.0.0'
    	}
	stage('Run Container on Prod Server'){
        def dockerRun = 'sudo docker run -p 5000:8080 -d sumand123/devopsprojects:2.0.0'
        sshagent(['pem-tomcatser']) {
            sh "ssh -o StrictHostKeyChecking=no ec2-user@3.16.15.121 ${dockerRun}"
	}
	}

}
