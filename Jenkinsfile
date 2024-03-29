/* groovylint-disable LineLength */
/* groovylint-disable-next-line LineLength */
/* groovylint-disable CompileStatic, ConsecutiveStringConcatenation, Indentation, NestedBlockDepth, SpaceBeforeOpeningBrace */
pipeline {
    agent any

    environment{
        DOCKERHUB_USERNAME = 'shankaru'
        APP_NAME = 'gitops-argocd-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + '/' + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages {
        stage('Cleanup workspace ') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM') {
            steps{
              script{
                git credentialsId: 'github',
                url: 'https://github.com/shankaru98/gitops_argocd_project.git',
                branch: 'main'
              }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('', REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
