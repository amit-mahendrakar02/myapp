pipeline {

    agent any

	environment {
        IMAGE_NAME = 'amitmahendrakar/myapp'
    }

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
		sh '''
			docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
			docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
		'''
        }
	}

	stage('Push to Docker Hub') {
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'dockerhub-credentials',
                usernameVariable: 'DOCKER_USERNAME',
                passwordVariable: 'DOCKER_PASSWORD'
            )
        ]) {
            sh '''
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin

                docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                docker push ${DOCKER_IMAGE}:latest

                docker logout
            '''
        }
    }
}


    }
	post {
	   success {
		echo 'CI pipeline completed Successfully'
	   }

	   failure {
		echo 'CI pipeline failed'
	   }

	}

}

