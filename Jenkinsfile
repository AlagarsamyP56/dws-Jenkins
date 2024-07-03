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
                        jarPath = bat(script: 'powershell -Command "Get-ChildItem target\\*.jar | Select-Object -First 1 | ForEach-Object { $_.FullName }"', returnStdout: true).trim()
                        jarPath = jarPath.split("\n").last().trim() // Extract the actual path from the command output
                    }

                  

                    def containerName = 'demo'
                   

                    // Stop and remove existing container
                    if (isUnix()) {
                        sh "docker stop ${containerName} || true"
                        sh "docker rm ${containerName} || true"
                    } else {
                        bat "docker stop ${containerName} || exit 0"
                        bat "docker rm ${containerName} || exit 0"
                    }

                    // Run the Docker container
                     {
                        bat "docker run -d --name ${containerName} -p 8082:8082 dockermule"
                    }

                    // Print a message indicating that the JAR file will be copied
                    echo "Copying JAR file to Docker container: ${jarPath}"

                    // Copy the JAR file to the Docker container
                   {
                        bat "docker cp \"${jarPath}\" ${containerName}:/opt/mule/apps"
                    }
                }
            }
        }
    }
}
