pipeline {
    agent any
    environment{
      dockerImage = ''
      registry = 'eu.gcr.io/k8s-training-260111/simple-app'
      registryCredentials = 'gcr:container-registry-credentials'
    }

    stages{
        stage('Cloning GIT'){
          steps{
            git 'https://github.com/piotrzalecki/jenkins-training.git'
          }
        }
        stage('Build docker image'){
            steps {
              dir('app'){
              script{
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
              }
            }
        }
        stage('Deploy Image'){
          steps{
            script{
              docker.withRegistry('https://eu.gcr.io', registryCredentials){dockerImage.push()}
            }
          }
        }
        stage('Remove Unused Docker Image'){
            steps{
                sh 'docker rmi $registry:$BUILD_NUMBER'
            }
        }

    }
}

// image-registry-jenkins@k8s-training-260111.iam.gserviceaccount.com
