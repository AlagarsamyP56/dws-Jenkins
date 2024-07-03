pipeline {
    agent any

    stages {
        stage('Build Application') {
            steps {
                script {
                    bat 'mvn clean install'
                }
            }
        }
        stage('Deploy to Docker') {
            steps {
                script {
                    // Get the JAR file path using PowerShell on Windows
                    def jarPath = bat(script: 'powershell -Command "Get-ChildItem target\\*.jar | Select-Object -First 1 | ForEach-Object { $_.FullName }"', returnStdout: true).trim()
                    jarPath = jarPath.split("\n").last().trim() // Extract the actual path from the command output

                    echo "Verified JAR file path: ${jarPath}"

                    def containerName = 'demo'
                    echo "Docker container name: ${containerName}"

                    // Find an available port
                    def port = bat(script: 'powershell -Command "(New-Object System.Net.Sockets.TcpListener([System.Net.IPAddress]::Any, 0)).LocalEndpoint.Port"', returnStdout: true).trim()

                    echo "Selected port: ${port}"

                    // Stop and remove existing container
                    bat "docker stop ${containerName} || exit 0"
                    bat "docker rm ${containerName} || exit 0"

                    // Run the Docker container with the dynamic port
                    bat "docker run -d --name ${containerName} -p ${port}:8082 dockermule"

                    // Print a message indicating that the JAR file will be copied
                    echo "Copying JAR file to Docker container: ${jarPath}"

                    // Copy the JAR file to the Docker container
                    bat "docker cp \"${jarPath}\" ${containerName}:/opt/mule/apps"

                    // Print a message with the dynamic port
                    echo "Application is running on port: ${port}"
                }
            }
        }
    }
}
