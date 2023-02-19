pipeline{

    agent any
    environment{
        DOCKERHUB_USERNAME = "assclic19"
        APP_NAME = "gitops-argocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CRED = 'dockerhub'
    }

    stages{
        stage('cleanup workspace'){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('checkout SCM'){
            steps{
                script{
                    git credentials: 'github',
                    url: 'https://github.com/assclic19/gitops-project.git',
                    branch: 'master'
                }
            }
        }
        stage('Docker image build'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Docker push Image'){
            steps{
                script{
                    docker.withRegistry('', REGISTRY_CRED){
                    docker_image.push("$BUILD_NUMBER")
                    docker_image.push "latest"
                    }
                }
            }
        }
        stage('delete Docker image'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
    }
}

