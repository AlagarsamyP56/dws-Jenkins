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
                    // Determine the JAR file path dynamically
                    def jarPath = ''
                    if (isUnix()) {
                        jarPath = sh(script: "ls target/*.jar", returnStdout: true).trim()
                    } else {
                        jarPath = bat(script: "for %i in (target\\*.jar) do @echo %i", returnStdout: true).trim()
                    }

                    // Define Docker container name
                    def containerName = 'jenkins-mule-api'
                    
                    // Run the Docker container
                    if (isUnix()) {
                        sh "docker run -d --name ${containerName} -p 8083:8083 dockermule"
                    } else {
                        bat "docker run -d --name ${containerName} -p 8083:8083 dockermule"
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
