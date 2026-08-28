pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building Spring Boot application'
                sh './mvnw clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh './mvnw test'
            }
        }

	stage('Docker Build'){
	    steps {
		echo 'Building Docker Image'
		sh ' docker build -t myapp:${BUILD_NUMBER} . '
            }
	}

    }
	post {
	   success {
		echo 'CI pipeline completed Successfully'
	   }

	   failure {
		echo 'CI pipeline failed
	   }

	}

}

