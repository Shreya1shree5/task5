pipeline {
    agent any
	
	
	environment {
         PROJECT_ID = 'solid-altar-444910-c9'
         CLUSTER_NAME = 'cluster-1'
         LOCATION = 'us-central1'
         CREDENTIALS_ID = 'kubernetes'	
	}
	
    stages {
	    stage('Scm Checkout') {
		    steps {
			    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Shreya1shree5/k8s.git/']])
		    }
		    }
	    
	 stage('Build Docker Image') {
		    steps {
			    sh 'whoami'
			    script {
				    myimageone = docker.build("shreya123shree/demo-testone:${env.BUILD_ID}")
			    }
		    }
	    }
	    
	    stage('Push Docker Image') {
		    steps {
			    script {
				    echo "Push Docker Image"
				    withCredentials([string(credentialsId: 'docker_id', variable: 'docker_id')]) {
            				sh "docker login -u shreya123shree -p ${docker_id}"
				    }
				        myimageone.push("${env.BUILD_ID}")
				    
			    }
		    }
	    }
	    
	    stage('Deploy to K8s') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
			    sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
			    echo "Start deployment of deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
			    echo "Deployment Finished ..."
		    }
	    }
    }
}
