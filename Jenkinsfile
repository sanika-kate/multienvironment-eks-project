pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Select environment')
    }

    environment {
        DOCKERHUB_CREDS = credentials('docker-creds')
        BACKEND_IMAGE = "sanikakate2002/backend"
        FRONTEND_IMAGE = "sanikakate2002/frontend"

        ENV_NAME = "${params.ENV}"                 // ✅ FIXED
        TAG = "${params.ENV}-${BUILD_NUMBER}"      // ✅ FIXED

        AWS_REGION = "us-east-2"
        CLUSTER_NAME = "cluster"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                sh '''
                set -e
                echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin
                '''
            }
        }

        stage('Build Images') {
            steps {
                sh '''
                set -e
                docker build -t $BACKEND_IMAGE:$TAG ./backend
                docker build -t $FRONTEND_IMAGE:$TAG ./frontend
                '''
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                set -e
                docker push $BACKEND_IMAGE:$TAG
                docker push $FRONTEND_IMAGE:$TAG
                '''
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'aws-creds',
                    usernameVariable: 'AWS_ACCESS_KEY_ID',
                    passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                )]) {
                    sh '''
                    set -e

                    export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                    export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY

                    aws eks update-kubeconfig --region $AWS_REGION --name $CLUSTER_NAME

                    echo "Deploying to environment: $ENV_NAME"

                    helm upgrade --install myapp ./my-app \
                    -f env/${ENV_NAME}-values.yaml \
                    --set backend.image.tag=$TAG \
                    --set frontend.image.tag=$TAG \
                    --namespace ${ENV_NAME} --create-namespace
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh '''
                docker rmi $BACKEND_IMAGE:$TAG || true
                docker rmi $FRONTEND_IMAGE:$TAG || true
                docker logout
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful for ${ENV_NAME} environment"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
