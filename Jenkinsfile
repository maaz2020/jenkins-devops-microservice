pipeline {
    agent any
    // agent {
    //      docker {
    //         image 'maven:3.6.3'
    //     }
    // }
    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'
        PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                sh 'mvn --version'
                sh 'docker version'
                echo 'Build'
                echo "PATH - $env.PATH"
                echo "BUILD_NUMBER - ${env.BUILD_NUMBER}"
                echo "BUILD_ID - ${env.BUILD_ID}"
                echo "BUILD_TAG - ${env.BUILD_TAG}"
                echo "BUILD_URL - ${env.BUILD_URL}"
                echo "JOB_NAME - ${env.JOB_NAME}"
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // sh 'mvn test'
                echo 'Test'
            }
        }

        stage('Integration Test') {
            steps {
                sh 'mvn failsafe:integration-test failsafe:verify'
            }
        }
         stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
          stage('Docker Build Image') {
            steps {
                script {
                    dockerImage = docker.build("warsiviews123/currency-exchange-devops:${env.BUILD_TAG}")
                }
            }
        }
          stage('Docker Push Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub'){
                    dockerImage.push()
                    dockerImage.push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'I am awesome. I run always.'
        }
        success {
            echo 'I run when you are successful.'
        }
        failure {
            echo 'I run when you fail.'
        }
    }
}
