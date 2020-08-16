pipeline {
	agent any
	// agent { docker { image 'maven:3.6.3'} }
	// agent { docker { image 'node:13.8'} }
	
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH="$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	
	stages {
		stage('checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
	      		echo "JOB_NAME - $env.JOB_NAME"
			}
		}
		stage('compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage('Integration Test') {
			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}
		stage('Package') {
			steps {
				sh "mvn package -DskipTest"
			}
		}
		stage('Build Docker Image') {
			steps {
				script {
					dockerImage = docker.build("naveelh/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}
		stage('Building docker container') {
			steps {
				sh "docker run --detach --name ${env.BUILD_NAME}:${env.BUILD_ID} --publish 8000:8000 naveelh/currency-exchange-devops:${env.BUILD_TAG}"
			}
		}
		stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry('', 'docker-hub-credentials') {
						dockerImage.push();
						dockerImage.push('latest');
					}
				}
			}
		}
	}
	
}
