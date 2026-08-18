pipeline{
    agent any
    environment{
        FRONTEND='tigershroff/deploy:frontend'
        BACKEND='tigershroff/deploy:backend'
    }
    stages{
        stage('checkout'){
            steps{
                git 'https://github.com/siddudev/Java-Full-Stack-App.git'
            }
        }
        stage('image creation'){
            steps{
                sh '''
                    docker build -t $FRONTEND$BUILD_ID ./frontend/.
                    docker build -t $BACKEND$BUILD_ID ./backend/.
                '''
            }
        }
        stage('push images'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub-cred', url: 'https://index.docker.io/v1/') {
                        sh '''
                            docker push $FRONTEND$BUILD_ID
                            docker push $BACKEND$BUILD_ID
                        '''
                    }
                }
            }
        }
        stage('stop existing containers'){
            agent{
                label 'Server'
            }
            steps{
                sh '''
                    docker stop frontend || true
                    docker stop backend || true
                '''
            }
        }
        stage('start containers'){
            agent{
                label 'Server'
            }
            steps{
                sh '''
                    docker run -d -p 3000:80 --name frontend $FRONTEND$BUILD_ID 
                    docker run -d -p 9090:8080 --name backend $BACKEND$BUILD_ID
                '''
            }
        }
        stage('clean-up'){
            agent{
                label 'Server'
            }            
            steps{
                sh 'echo y | docker system prune -a'
            }
        }
    }
}
