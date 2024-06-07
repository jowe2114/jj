@Library('Jowe-shared-library') _

agent {
        label 'slave'
    }
pipeline {
    environment {
        imageName = 'jowe2114/ivolve-jenkins-app'
    }

    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }


        stage('build') {
            steps {
               script{
                    sh '   chmod +x gradlew  '
                    sh '  ./gradlew build '
                
               }
            }
        }

        stage('test') {
            steps {
               script{
                    sh '  chmod +x gradlew  '
                    sh '  ./gradlew test '
                
               }
            }
        }





        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    buildPushtoHub([ image: "${imageName}:${BUILD_NUMBER}", DockerCredentials: 'DOCKERHUB' ])
                } 
            }
        }
                stage('kubernetes') {
            steps {
               withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: '', contextName: '', credentialsId: '4', namespace: 'omaryousef', serverUrl: 'https://api.ocp-training.ivolve-test.com:6443']]) {    
                 sh 'kubectl apply -f DeplymentAndSvc.yml -n omaryousef'
                 
            }
            }
        }






    }
}
