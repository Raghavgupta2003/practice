pipeline {
    agent any

    stages {
        stage('Docker Cleanup') {
            steps {
                bat '''
                docker stop html || echo Container not running
                docker rm html || echo No container to remove
                docker rmi -f html || echo No image to remove
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t html .'
            }
        }
        stage('Run Container') {
            steps {
                bat 'docker run -d -p 8090:80 --name html html'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        docker tag html %DOCKER_USER%/html
                        docker push %DOCKER_USER%/html
                    '''
                }
            }
        }        
    }
}
