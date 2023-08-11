pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '20'))
        disableConcurrentBuilds()
        timeout(time: 60, unit: 'MINUTES')
        timestamps()
    }
    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-creds')
    } 
    stages {
        stage('SonarQube analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:4.7.0'
                }
            }
            environment {
                CI = 'true'
                scannerHome = '/opt/sonar-scanner'
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        } 
         stage('Build auth') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        cd auth
                        docker build -t devopseasylearning/s4gautier-auth:${BUILD_NUMBER} .
                        cd -
                    '''
                }
            }
        }

        stage('push auth') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        docker push devopseasylearning/s4gautier-auth:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Build db') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        cd DB
                        docker build -t devopseasylearning/s4gautier-db:${BUILD_NUMBER} .
                        cd -
                    '''
                }
            }
        }

        stage('push db') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        docker push devopseasylearning/s4gautier-db:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Build ui') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        cd UI
                        docker build -t devopseasylearning/s4gautier-ui:${BUILD_NUMBER} .
                        cd -
                    '''
                }
            }
        }

        stage('push ui') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        docker push devopseasylearning/s4gautier-ui:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Build weather') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        cd weather
                        docker build -t devopseasylearning/s4gautier-weather:${BUILD_NUMBER} .
                        cd -
                    '''
                }
            }
        }

        stage('push weather') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh '''
                        docker push devopseasylearning/s4gautier-weather:${BUILD_NUMBER}
                    '''
                }
            }
        }


    }

     post {
   
   success {
      slackSend (channel: '#development-alerts', color: 'good', message: "SUCCESSFUL: Application s4gautier-pipeline  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }

 
    unstable {
      slackSend (channel: '#development-alerts', color: 'warning', message: "UNSTABLE: Application s4gautier-pipeline  Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }

    failure {
      slackSend (channel: '#development-alerts', color: '#FF0000', message: "FAILURE: Application s4gautier-pipeline Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
   
    cleanup {
      deleteDir()
    }
}
}

        







       