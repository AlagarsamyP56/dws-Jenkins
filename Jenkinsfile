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
                    def jarPath = ''
                    if (isUnix()) {
                        // Get the JAR file path
                        jarPath = sh(script: "ls target/*.jar | head -n 1", returnStdout: true).trim()
                        // Print the JAR file path for debugging
                        echo "JAR file path: ${jarPath}"
                    } else {
                        // Get the JAR file path
                        jarPath = bat(script: 'for %i in (target\\*.jar) do @echo %i', returnStdout: true).split('\r\n').find { it.contains('.jar') }.trim()
                        // Print the JAR file path for debugging
                        echo "JAR file path: ${jarPath}"
                    }

                    def containerName = 'jenkins-mule-api'

                    // Print the container name for debugging
                    echo "Docker container name: ${containerName}"

                    // Run the Docker container
                    if (isUnix()) {
                        sh "docker run -d --name ${containerName} -p 8083:8083 dockermule"
                    } else {
                        bat "docker run -d --name ${containerName} -p 8083:8083 dockermule"
                    }

                    // Print a message indicating that the JAR file will be copied
                    echo "Copying JAR file to Docker container: ${jarPath}"

                    // Copy the JAR file to the Docker container
                    if (isUnix()) {
                        sh "docker cp ${jarPath} ${containerName}:/opt/mule/apps"
                    } else {
                        bat "docker cp \"${jarPath}\" ${containerName}:/opt/mule/apps"
                    }
                }
            }
        }
    }
}
