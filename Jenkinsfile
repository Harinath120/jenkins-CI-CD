pipeline {
	agent any
	//agent { docker { image 'node:13.8'} }
	environment {
		dockerHome = tool 'mydocker'
		mavenHome = tool 'mymaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		
		stage('checkout') {
			steps {
				sh 'mvn -version'
				sh 'docker version'
				echo "Build"
				echo "PATH -$PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
				echo "BUILD_URL - $env.BUILD_URL"
			}
		}
		stage('compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage('Test') {
			steps {
				sh "mvn test"
			}
		}
		stage('Integration Test') {
			steps {
				sh "mvn failsafe:Integration test failsafe:verify"
			}
		}
	}
	post {
		always {
			echo 'Iam awesome. I run always'
		}
		success {
			echo ' I run when you are successful'
		}
		failure {
			echo 'I run when you are fail'
		}
	}
	
}

