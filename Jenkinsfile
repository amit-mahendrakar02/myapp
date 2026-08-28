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

    }

}

