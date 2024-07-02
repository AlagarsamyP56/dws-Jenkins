pipeline {
    agent any

    stages {
        stage('Build Application') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'
                    } else {
                        bat 'mvn clean install'
                    }
                }
            }
        }
        stage('Deploy to Docker') {
            steps {
                script {
                    def jarPath = 'target/agin-1.0.0-SNAPSHOT-mule-application.jar'
                    def containerName = 'jenkins-mule-api'
                    
                    // Run the Docker container
                    if (isUnix()) {
                        sh "docker run -d --name ${containerName} -p 8081:8081 dockermule"
                    } else {
                        bat "docker run -d --name ${containerName} -p 8081:8081 dockermule"
                    }
                    
                    // Copy the JAR file to the Docker container
                    if (isUnix()) {
                        sh "docker cp ${jarPath} ${containerName}:/opt/mule/apps"
                    } else {
                        bat "docker cp ${jarPath} ${containerName}:/opt/mule/apps"
                    }
                }
            }
        }
    }
}

