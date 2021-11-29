pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git '/home/simple-node-js-react-npm-app'
			}
		}

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
			}
		}
	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}