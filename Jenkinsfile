pipeline {
    agent any
	
	environment {
		   PROJECT_ID = 'solid-altar-444910-c9'
           CLUSTER_NAME = 'cluster-1'
           LOCATION = 'us-central1-c'
           CREDENTIALS_ID = 'My First Project'		
	}

	
    stages {
	    stage('Scm Checkout') {
		    steps {
			    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Shreya1shree5/task5.git']])
		    }
	    }
    stage('Build Docker Image') {
		    steps {
			    sh 'whoami'
			    script {
				    myimage = docker.build("shreya123shree/demo-test:${env.BUILD_ID}")
			    }
		    }
	    }
    stage("Push Docker Image") {
		    steps {
			    script {
				    echo "Push Docker Image"
				    withCredentials([string(credentialsId: 'docker_id', variable: 'docker_id')]) {
            				sh "docker login -u shreya123shree -p ${docker_id}"
				    }
				        myimage.push("${env.BUILD_ID}")
				    
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
