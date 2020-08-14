pipeline {
	agent any
	agent { docker { image 'maven:3.6.3'} }
	agent { docker { image 'node:13.8'} }
	
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH="$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	
	stage('checkout') {
		steps{
			sh 'mvn --version'
			sh 'docker version'
			echo "Build"
			echo "PATH - $PATH"
			echo "BUILD_NUMBER - $env.BUILD_NUMBER"
			echo "BUILD_ID - $env.BUILD_ID"
			echo "JOB_NAME - $env.JOB_NAME"
		}
	}
}