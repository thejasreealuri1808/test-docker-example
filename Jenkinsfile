pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh './mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t spring-petclinic .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh '''
                    echo $PASSWORD | docker login -u $USERNAME --password-stdin
                    docker tag spring-petclinic $USERNAME/spring-petclinic:latest
                    docker push $USERNAME/spring-petclinic:latest
                    '''
                }
            }
        }
    }
}
