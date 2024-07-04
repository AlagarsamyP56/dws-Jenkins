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

                    // Set the Docker container name based on the pipeline job name
                    def containerName = env.JOB_NAME
                    echo "Docker container name: ${containerName}"

                    // Stop and remove existing container
                    bat "docker stop ${containerName} || exit 0"
                    bat "docker rm ${containerName} || exit 0"

                    // Run the Docker container
                    bat "docker run -d --name ${containerName} -p 8081:8081 dockermule"

                    // Print a message indicating that the JAR file will be copied
                    echo "Copying JAR file to Docker container: ${jarPath}"

                    // Copy the JAR file to the Docker container
                    bat "docker cp \"${jarPath}\" ${containerName}:/opt/mule/apps"
                }
            }
        }
    }
}
