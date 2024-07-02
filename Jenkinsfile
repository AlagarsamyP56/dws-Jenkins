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
        stage('Prepare Docker') {
            steps {
                script {
                    // Build Docker image with the MuleSoft runtime
                    sh 'docker build -t my-mule-app .'
                }
            }
        }
        stage('Deploy to Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh """
                    docker run -d --name mule-container -v /path/to/your/local/m2/repository:/root/.m2 my-mule-app
                    """

                    // Copy the JAR file from the local filesystem to the Docker container
                    sh """
                    docker cp target/dws-jenkins-1.0.0-SNAPSHOT-mule-application.jar mule-container:/opt/mule-enterprise-standalone-4.1.5/apps/
                    """

                    // Start the Mule application inside the Docker container
                    sh """
                    docker exec mule-container /opt/mule-enterprise-standalone-4.1.5/bin/mule
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker containers
            sh 'docker rm -f mule-container'
        }
    }
}
