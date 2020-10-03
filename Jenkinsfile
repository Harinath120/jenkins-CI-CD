pipeline {
	agent any
	//agent { docker { image 'maven:3.6.3'} }
	//agent { docker { image 'node:13.8'} }
	environment {
		dockerHome = tool 'mydocker'
		mavenHome = tool 'mymaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
		registryCredential ='dockerhub'
	}


	stages {
		stage('Checkout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL -$env.BUILD_URL"
			}
		}
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}

		stage('Test') {
			steps {
				echo "mvn test"
			}
		}

		stage('Integration Test') {
			steps {
				echo "mvn failsafe:integration-test failsafe:verify"
			}
		}
		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}

		stage('Build Docker Image') {
			steps {
				//"docker build -t in28min/currency-exchange-devops:$env.BUILD_TAG"
				script {
					dockerImage = docker.build("in28min/currency-exchange-devops")
					dockerImage = docker.tag ("in28min/currency-exchange-devops harinath150/currency-exchange-devops")
				}
			}
		}

		stage('Push Docker Image') {
			steps {
				script {
					withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
						sh "docker login -u ${USERNAME} -p ${PASSWORD}"
						sh "docker push harinath150/currency-exchange-devops:jenkins-devops-microservice-pipeline-$env.BUILD_NUMBER"
					}			
                    
				}	    	

			}
		}
		
	}

	post {
		always {
			echo 'Iam awesome. I run always'
		}

		success {
			echo 'I run when you are successful'
		}

		failure {
			echo 'I run when you are fail'
		}
	}
}