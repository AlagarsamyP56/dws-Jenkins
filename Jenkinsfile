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
                        jarPath = sh(script: "ls target/*.jar | head -n 1", returnStdout: true).trim()
                        echo "JAR file path: ${jarPath}"
                    } else {
                        jarPath = bat(script: 'powershell -Command "Get-ChildItem target\\*.jar | Select-Object -First 1 | ForEach-Object { $_.FullName }"', returnStdout: true).trim()
                        echo "JAR file path: ${jarPath}"
                    }

                    echo "Verified JAR file path: ${jarPath}"

                    // Verify JAR file existence
                    if (isUnix()) {
                        sh "ls -l ${jarPath}"
                    } else {
                        bat "dir \"${jarPath}\""
                    }

                    def containerName = 'jenkins-mule-api'
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
                        sh "docker run -d --name ${containerName} -p 8083:8083 dockermule"
                    } else {
                        bat "docker run -d --name ${containerName} -p 8083:8083 dockermule"
                    }

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
