pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '20'))
        disableConcurrentBuilds()
        timeout (time: 60, unit: 'MINUTES')
        timestamps()
    }
    environment {
		DOCKERHUB_CREDS=credentials('dockerhub-creds')
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
        //  scannerHome = tool 'Sonar'
        scannerHome='/opt/sonar-scanner'
    }
            steps{
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        } 
        stage('Docker Login') {
                steps {
                    script {
                        // Log in to Docker Hub
                        sh "docker login -u ${DOCKERHUB_CREDS_USR} -p ${DOCKERHUB_CREDS_PSW}"
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
}
