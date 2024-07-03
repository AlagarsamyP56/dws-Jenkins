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
                        // Get the JAR file path on Unix
                        jarPath = sh(script: "ls target/*.jar | head -n 1", returnStdout: true).trim()
                    } else {
                        // Get the JAR file path using PowerShell on Windows
                        jarPath = bat(script: 'powershell -Command "Get-ChildItem target\\*.jar | Select-Object -First 1 | ForEach-Object { $_.FullName }"', returnStdout: true).trim()
                        jarPath = jarPath.split("\n").last().trim() // Extract the actual path from the command output
                    }

                    echo "Verified JAR file path: ${jarPath}"

                    def containerName = 'demo'
                    echo "Docker container name: ${containerName}"

                    // Stop and remove existing container
                    if (isUnix()) {
                        sh "docker stop ${containerName} || true"
                        sh "docker rm ${containerName} || true"
                    } else {
                        bat "docker stop ${containerName} || exit 0"
                        bat "docker rm ${containerName} || exit 0"
                    }

                    // Run the Docker container
                    if (isUnix()) {
                        sh "docker run -d --name ${containerName} -p 8081:8081 dockermule"
                    } else {
                        bat "docker run -d --name ${containerName} -p 8081:8081 dockermule"
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
