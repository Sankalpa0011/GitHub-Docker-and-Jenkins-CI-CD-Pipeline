pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Sankalpa0011/test-node'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'samin-docker', variable: 'samindocker')]) {
                    script {
                        bat "docker login -u dockersankalpa0011 -p %samindocker%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
