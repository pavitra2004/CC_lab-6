pipeline {
    agent any

    stages {

        stage('Create Network') {
            steps {
                sh '''
                docker network create lab6-network || true
                '''
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh '''
                docker rm -f backend1 backend2 || true
                docker run -d --network lab6-network --name backend1 backend-app
                docker run -d --network lab6-network --name backend2 backend-app
                '''
            }
        }

        stage('Deploy NGINX') {
            steps {
                sh '''
                docker rm -f nginx-lb || true
                docker run -d --network lab6-network --name nginx-lb -p 80:80 nginx
                sleep 3
                docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
                docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }
}
