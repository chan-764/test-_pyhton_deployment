pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "docker.io"
        IMAGE_NAME = "back_end/python3"
        IMAGE_TAG = "latest"
        DOCKER_CREDENTIALS = "docker"
    }
    triggers {
        //git('main') {  // Trigger when commits are made to the 'main' branch in the repository
            githubPush()
        }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git', url: 'https://github.com/chan-269/docker-course-three-tier-web-app.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker images for API, App, and DB
                    def image = docker.build("docker.io/chandanuikey19/backup_api_backend:latest", "./api")
                    docker.withRegistry('https://index.docker.io/v1/', "docker") {
                        image.push()
                    }

                    def front_image = docker.build("docker.io/chandanuikey19/backup_app_frontend:latest", "./app")
                    docker.withRegistry('https://index.docker.io/v1/', "docker") {
                        front_image.push()
                    }

                    def db_image = docker.build("docker.io/chandanuikey19/backup_mongodb_database:latest", "./db")
                    docker.withRegistry('https://index.docker.io/v1/', "docker") {
                        db_image.push()
                    }
                }
            }
        }

        // Optionally, you can add a cleanup step to remove unused Docker images
        // stage('Cleanup') {
        //     steps {
        //         sh 'docker system prune -f'
        //     }
        // }
    }

    // Post actions
    post {
        success {
            echo "Docker images pushed successfully."
        }
        failure {
            echo " Pipeline failed please check and correct successfully."
        }
    }
}


// pipeline {
//     agent any

//     environment {
//         CLUSTER_NAME = 'three-tier-cluster'
//         AWS_REGION = 'ap-south-1'
//         KUBECONFIG = "${WORKSPACE}/kubeconfig"
//         NAMESPACE = 'workshop'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 // Use credentials for private repository
//                 checkout([
//                     $class: 'GitSCM',
//                     branches: [[name: '*/main']], // Use 'master' if applicable
//                     userRemoteConfigs: [[url: 'https://github.com/chan-764/pyhtondeployment.git', credentialsId: 'your-credentials-id']]
//                 ])
//             }
//         }

//         stage('Setup AWS CLI and kubectl') {
//             steps {
//                 script {
//                     sh """
//                     aws eks update-kubeconfig --region ${AWS_REGION} --name ${CLUSTER_NAME} --kubeconfig ${KUBECONFIG}
//                     """
//                 }
//             }
//         }

//         stage('Create Namespace') {
//             steps {
//                 script {
//                     sh """
//                     kubectl --kubeconfig=${KUBECONFIG} apply -f - <<EOF
//                     apiVersion: v1
//                     kind: Namespace
//                     metadata:
//                       name: ${NAMESPACE}
//                     EOF
//                     """
//                 }
//             }
//         }

//         stage('Deploy Database') {
//             steps {
//                 script {
//                     sh """
//                     kubectl --kubeconfig=${KUBECONFIG} apply -f k8s/database-deploy.yaml -n ${NAMESPACE}
//                     """
//                 }
//             }
//         }

//         stage('Deploy Backend') {
//             steps {
//                 script {
//                     sh """
//                     kubectl --kubeconfig=${KUBECONFIG} apply -f k8s/backend-deploy.yaml -n ${NAMESPACE}
//                     """
//                 }
//             }
//         }

//         stage('Deploy Frontend') {
//             steps {
//                 script {
//                     sh """
//                     kubectl --kubeconfig=${KUBECONFIG} apply -f k8s/frontend-deploy.yaml -n ${NAMESPACE}
//                     """
//                 }
//             }
//         }

//         stage('Verify Deployments') {
//             steps {
//                 script {
//                     sh """
//                     kubectl --kubeconfig=${KUBECONFIG} get all -n ${NAMESPACE}
//                     """
//                 }
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Deployment Successful!'
//         }
//         failure {
//             echo 'Deployment Failed!'
//         }
//     }
// }


























