// pipeline {
//     agent any

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Build and Dockerize') {
//             steps {
//                 sh 'docker build -t my-node-app .'
//             }
//         }

//         stage('Push Docker Image to Registry') {
//             steps {
//                 withDockerRegistry([credentialsId: 'your-dockerhub-credentials', url: 'https://index.docker.io/v1/']) {
//                     sh 'docker push my-node-app'
//                 }
//             }
//         }

//         stage('Deploy to Elastic Beanstalk') {
//             steps {
//                 sh 'eb init -p node.js my-node-app'  // Initialize Elastic Beanstalk
//                 sh 'eb deploy my-node-app-env'  // Deploy to the desired environment
//             }
//         }
//     }
// }




// pipeline {
//     agent any
    
//     environment {
//         NODE_VERSION = '14'  // Choose the desired Node.js version
//         AWS_REGION = 'us-east-1'  // Set your AWS region
//         EB_ENV_NAME = 'your-env-name'  // Set your Elastic Beanstalk environment name
//     }
    
//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }
        
//         stage('Build and Push Docker Image') {
//             steps {
//                 script {
//                     def imageTag = "my-nodejs-app:${env.BUILD_NUMBER}"
//                     sh "docker build -t $imageTag ."
//                     sh "docker login -u AWS -p $(aws ecr get-login-password --region $AWS_REGION) <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.$AWS_REGION.amazonaws.com"
//                     sh "docker tag $imageTag <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.$AWS_REGION.amazonaws.com/$imageTag"
//                     sh "docker push <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.$AWS_REGION.amazonaws.com/$imageTag"
//                 }
//             }
//         }
        
//         stage('Deploy to Elastic Beanstalk') {
//             steps {
//                 script {
//                     def awsEbCli = tool name: 'AWS EB CLI', type: 'Tool'
//                     sh "${awsEbCli}/eb init -r $AWS_REGION -p nodejs"
//                     sh "${awsEbCli}/eb use $EB_ENV_NAME"
//                     sh "${awsEbCli}/eb deploy"
//                 }
//             }
//         }
//     }
// }



pipeline {
    agent any  // Use any available Jenkins agent

    environment {
        NODE_VERSION = '14'  // Specify the Node.js version to use
        AWS_REGION = 'us-east-1'  // Set the AWS region
        EB_ENV_NAME = 'your-env-name'  // Set the Elastic Beanstalk environment name
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Get the source code from your GitHub repository
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Create a unique tag for the Docker image based on the Jenkins build number
                    def imageTag = "my-nodejs-app:${env.BUILD_NUMBER}"
                    
                    // Build the Docker image using the specified Node.js version
                    sh "docker build -t $imageTag ."
                    
                    // Log in to the AWS Elastic Container Registry (ECR) using AWS credentials
                    sh "docker login -u AWS -p $(aws ecr get-login-password --region $AWS_REGION) <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.$AWS_REGION.amazonaws.com"
                    
                    // Tag the Docker image with the ECR repository URL
                    sh "docker tag $imageTag <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.$AWS_REGION.amazonaws.com/$imageTag"
                    
                    // Push the Docker image to the ECR repository
                    sh "docker push <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.$AWS_REGION.amazonaws.com/$imageTag"
                }
            }
        }
        
        stage('Deploy to Elastic Beanstalk server') {
            steps {
                script {
                    // Use the AWS Elastic Beanstalk Command Line Interface (EB CLI)
                    def awsEbCli = tool name: 'AWS EB CLI', type: 'Tool'
                    
                    // Initialize the Elastic Beanstalk environment
                    sh "${awsEbCli}/eb init -r $AWS_REGION -p nodejs"
                    
                    // Use the specified Elastic Beanstalk environment
                    sh "${awsEbCli}/eb use $EB_ENV_NAME"
                    
                    // Deploy the application to Elastic Beanstalk
                    sh "${awsEbCli}/eb deploy"
                }
            }
        }
    }
}
