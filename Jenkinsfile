pipeline{
    agent { label 'testserver'}
    stages('Deploy a PHP application'){
        stage('Cleanup Docker Environment') {
            steps {
                // Stop and remove any existing container named 'php'
                sh '''
                docker ps -a -q --filter "name=phpweb" | grep -q . && docker stop php && docker rm phpweb || echo "No container to remove"
                '''
                // Remove any existing image with the same tag
                sh '''
                docker images -q divyame91/mylearnings24:phpwebsite | grep -q . && docker rmi -f divyame91/mylearnings24:phpwebsite || echo "No image to remove"
                '''
            }
        }
        stage('Build a Docker Image'){
            steps{
               sh 'docker build -t divyame91/mylearnings24:phpwebsite .'
            }
        }
        stage('Run Docker Container'){
            steps{
                sh 'docker run -dit --name phpweb -p 80:80 divyame91/mylearnings24:phpwebsite'
            }
        }
        stage('Deploy to Kubernetes via Ansible') {
            steps {
                script {
                    sh 'ansible-playbook php_deploy.yml'
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
