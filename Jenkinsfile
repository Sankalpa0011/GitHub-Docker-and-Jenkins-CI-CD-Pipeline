// // pipeline {
// //     agent any 
    
// //     stages { 
// //         stage('SCM Checkout') {
// //             steps {
// //                 retry(3) {
// //                     git branch: 'main', url: 'https://github.com/Sankalpa0011/GitHub-Docker-and-Jenkins-CI-CD-Pipeline',
// //                     credentialsId: 'github-token'
// //                 }
// //             }
// //         }
// //         stage('Build Docker Image') {
// //             steps {  
// //                 bat 'docker build -t dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER% .'
// //             }
// //         }
// //         stage('Login to Docker Hub') {
// //             steps {
// //                 withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test-dockerhubpassword')]) {
// //                     script {
// //                         bat "docker login -u dockersankalpa0011 -p %test-dockerhubpassword%"
// //                     }
// //                 }
// //             }
// //         }
// //         stage('Push Image') {
// //             steps {
// //                 bat 'docker push dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER%'
// //             }
// //         }
// //     }
// //     post {
// //         always {
// //             bat 'docker logout'
// //         }
// //     }
// // }
        
        
        
        
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





// pipeline {
//     agent any 
    
//     stages { 
//         // Step 1: Checkout Code from GitHub
//         stage('SCM Checkout') {
//             steps {
//                 retry(3) {
//                     git branch: 'main', url: 'https://github.com/Sankalpa0011/GitHub-Docker-and-Jenkins-CI-CD-Pipeline',
//                     credentialsId: 'github-token'
//                 }
//             }
//         }

//         // Step 2: Build Docker Image
//         stage('Build Docker Image') {
//             steps {  
//                 bat 'docker build -t dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER% .'
//             }
//         }

//         // Step 3: Login to Docker Hub
//         stage('Login to Docker Hub') {
//             steps {
//                 withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test-dockerhubpassword')]) {
//                     script {
//                         bat "docker login -u dockersankalpa0011 -p %test-dockerhubpassword%"
//                     }
//                 }
//             }
//         }

//         // Step 4: Push Docker Image to Docker Hub
//         stage('Push Image') {
//             steps {
//                 bat 'docker push dockersankalpa0011/nodeapp-cuban:%BUILD_NUMBER%'
//             }
//         }

//         // Step 5: Deploy Docker Image on AWS EC2
//         stage('Deploy to AWS EC2') {
//             steps {
//                 script {
//                     def ec2InstanceIP = '51.20.12.144'  // Replace with EC2 public IP
//                     def sshUser = 'ubuntu'                    // Replace with correct SSH user
//                     def deployCommand = """
//                         docker pull dockersankalpa0011/nodeapp-cuban:${BUILD_NUMBER} && \
//                         docker stop nodeapp-container || true && \
//                         docker rm nodeapp-container || true && \
//                         docker run -d --name nodeapp-container -p 80:80 dockersankalpa0011/nodeapp-cuban:${BUILD_NUMBER}
//                     """
                    
//                     // Running the command on EC2
//                     sshPublisher(publishers: [
//                         sshPublisherDesc(
//                             configName: 'EC2-SSH-Config', 
//                             transfers: [sshTransfer(execCommand: deployCommand)],
//                             usePromotionTimestamp: false, alwaysPublishFromMaster: true
//                         )
//                     ])
//                 }
//             }
//         }
//     }

//     // Logout from Docker Hub after the job is done
//     post {
//         always {
//             bat 'docker logout'
//         }
//     }
// }
