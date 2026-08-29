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
                withDockerRegistry(
                    credentialsId: 'dockerhub-credentials',
                    url: 'https://index.docker.io/v1/'
                ) {
                    sh '''
                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                        docker push ${IMAGE_NAME}:latest
                    '''
                }
            }
        }


    }
	post {
		always {
        sh '''
            docker rmi ${IMAGE_NAME}:${BUILD_NUMBER} || true
            docker rmi ${IMAGE_NAME}:latest || true
            docker image prune -f
        '''
	   success {
		echo 'CI pipeline completed Successfully'
	   }

	   failure {
		echo 'CI pipeline failed'
	   }

	}

}

