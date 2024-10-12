// pipeline {
//     agent any 
    
//     stages { 
//         stage('SCM Checkout') {
//             steps {
//                 retry(3) {
//                     git branch: 'main', url: 'https://github.com/Sankalpa0011/GitHub-Docker-and-Jenkins-CI-CD-Pipeline',
//                     credentialsId: 'github-token'
//                 }
//             }
//         }
//         stage('Build Docker Image') {
//             steps {  
//                 bat 'docker build -t dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER% .'
//             }
//         }
//         stage('Login to Docker Hub') {
//             steps {
//                 withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test-dockerhubpassword')]) {
//                     script {
//                         bat "docker login -u dockersankalpa0011 -p %test-dockerhubpassword%"
//                     }
//                 }
//             }
//         }
//         stage('Push Image') {
//             steps {
//                 bat 'docker push dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER%'
//             }
//         }
//     }
//     post {
//         always {
//             bat 'docker logout'
//         }
//     }
// }
        
        
        
        
pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Sankalpa0011/GitHub-Docker-and-Jenkins-CI-CD-Pipeline',
                    credentialsId: 'github-token'
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
                withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test-dockerhubpassword')]) {
                    script {
                        bat "docker login -u dockersankalpa0011 -p %test-dockerhubpassword%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
        stage('Deploy to AWS EC2') {
            steps {
                script {
                    // Define your EC2 instance details
                    def ec2InstanceIP = '51.20.12.144' // Replace with your EC2 public IP
                    def sshUser = 'ubuntu' // Or your respective user

                    // Command to run on the EC2 instance
                    def deployCommand = """
                        docker pull dockersankalpa0011/nodeapp-cuban:${BUILD_NUMBER} && \
                        docker stop nodeapp-container || true && \
                        docker rm nodeapp-container || true && \
                        docker run -d --name nodeapp-container -p 80:80 dockersankalpa0011/nodeapp-cuban:${BUILD_NUMBER}
                    """
                    
                    // Run the command on EC2 using SSH
                    sshCommand remote: ec2InstanceIP, user: sshUser, command: deployCommand, credentialsId: 'ec2-ssh-credentials'
                }
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}

