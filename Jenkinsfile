pipeline {

    agent any

    tools {
        jdk 'JDK-25'
        maven 'Maven-3.10'
    }

    stages {

        stage('SCM Synchronization') {
            steps {
                echo 'Pulling latest code changes from Git repository...'
                checkout scm
            }
        }

        stage('Static Verification') {
            steps {
                echo 'Checking Maven project layout rules...'
                bat 'mvn validate'
            }
        }

        stage('Compilation & Automated Testing') {
            steps {
                echo 'Compiling and running automated tests...'
                bat 'mvn clean package'
            }

            post {
                always {
                    echo 'Publishing Unit Test results to Jenkins Dashboard...'
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Starting SonarQube code quality analysis...'

                withSonarQubeEnv('SonarQube-Local') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

    }

    post {
        success {
            echo 'CI Pipeline completed successfully! Code stability confirmed.'
        }

        failure {
            echo 'CI Pipeline failed. Check the console logs.'
        }
    }

}