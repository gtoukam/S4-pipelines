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
                        docker build -t devopseasylearning/s4-pipelines-auth:${BUILD_NUMBER} .
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
                        docker push devopseasylearning/s4-pipelines-auth:${BUILD_NUMBER}
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
                        docker build -t devopseasylearning/s4-pipelines-db:${BUILD_NUMBER} .
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
                        docker push devopseasylearning/s4-pipelines-db:${BUILD_NUMBER}
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
                        docker build -t devopseasylearning/s4-pipelines-ui:${BUILD_NUMBER} .
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
                        docker push devopseasylearning/s4-pipelines-ui:${BUILD_NUMBER}
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
                        docker build -t devopseasylearning/s4-pipelines-weather:${BUILD_NUMBER} .
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
                        docker push devopseasylearning/s4-pipelines-weather:${BUILD_NUMBER}
                    '''
                }
            }
        }


    }
}

        







        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean install'
            }
        }
        
        stage('Test') {
            steps { 
                // Run tests
                sh 'mvn test'

            }
        }
    
    post {
        always {
            // Clean up after the build, regardless of success or failure
            deleteDir()
        }
        success {
            // Actions to perform if the build is successful
            echo 'Build successful!'
        }
        failure {
            // Actions to perform if the build fails
            echo 'Build failed!'
        }
    }

