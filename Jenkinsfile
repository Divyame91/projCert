pipeline{
    agent { label 'testserver'}
    stages('Deploy a PHP application'){
        stage('Cleanup Docker Environment') {
            steps {
                sh '''
                docker ps -a -q --filter "name=php" | grep -q . && docker stop php && docker rm php || echo "No container to remove"
                '''
                sh '''
                docker images -q divyame91/mylearnings24:phpwebapp | grep -q . && docker rmi -f divyame91/mylearnings24:phpwebapp || echo "No image to remove"
                '''
            }
        }
        stage('Build a Docker Image'){
            steps{
               sh 'docker build -t divyame91/mylearnings24:phpwebapp .'
            }
        }
        stage('Run Docker Container'){
            steps{
                sh 'docker run -dit --name php -p 80:80 divyame91/mylearnings24:phpwebapp'
            }
        }
    }
     post {
        failure {
            steps {
                echo "Job failed. Cleaning up non-running Docker containers..."
                sh 'docker container prune -f'
            }
        }
}
}
