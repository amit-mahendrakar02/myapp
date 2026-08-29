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
                set -e

                echo "Pushing ${IMAGE_NAME}:${BUILD_NUMBER}"

                echo "$DOCKER_PASSWORD" | docker login \
                    -u "$DOCKER_USERNAME" \
                    --password-stdin

                docker push "${IMAGE_NAME}:${BUILD_NUMBER}"
                docker push "${IMAGE_NAME}:latest"

                docker logout
            '''
        }
    }
}

	
	stage('Deploy to Kubernetes') {
		steps {
		echo 'Deploying application to Kubernetes'
		
		sh '''
			kubectl -f apply k8s/deployment.yml
			kubectl -f apply k8s/service.yml

			kubectl rollout restart deployment/myapp
			kubectl rollout status deployment/myapp
		'''
	}
	}

    }
	// post {
		// always {
  //       sh '''
  //           docker rmi ${IMAGE_NAME}:${BUILD_NUMBER} || true
  //           docker rmi ${IMAGE_NAME}:latest || true
  //           docker image prune -f
  //       '''
		// }
	   success {
		echo 'CI pipeline completed Successfully'
	   }

	   failure {
		echo 'CI pipeline failed'
	   }

	}

}

