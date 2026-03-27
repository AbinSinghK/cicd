pipeline {
    agent any

    environment {
        IMAGE = 'abin2001/portfolio:v1'   // Docker Hub image
        CONTAINER_NAME = 'portfolio-app'
    }

    stages {
        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER docker.io --password-stdin
                    '''
                }
            }
        }
        
        stage('Stop Old Container') {
            steps {
                sh '''
                if [ "$(docker ps -a -q -f name=$CONTAINER_NAME)" ]; then
                  docker stop $CONTAINER_NAME || true
                  docker rm $CONTAINER_NAME || true
                fi
                '''
            }
        }

        stage('Pull Image') {
            steps {
                sh 'docker pull $IMAGE'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --name $CONTAINER_NAME -p 80:80 $IMAGE'
            }
        }
    }
}
