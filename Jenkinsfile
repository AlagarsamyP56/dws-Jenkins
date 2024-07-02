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
        stage('Deploy CloudHubs') {
            environment {
                ANYPOINT_CREDENTIALS = credentials('anypoint.hrms')
            }
            steps {
                echo 'Deploying mule project due to the latest code commit…'
                echo 'Deploying to the configured environment….'
                script {
                    def username = alagar12345678
                    def password = Good@123
                    if (isUnix()) {
                        sh "mvn clean deploy -DmuleDeploy -Dusername=${username} -Dpassword=${password} -DworkerType=Micro -Dworkers=1"
                    } else {
                        bat "mvn clean deploy -DmuleDeploy -Dusername=${username} -Dpassword=${password} -DworkerType=Micro -Dworkers=1"
                    }
                }
            }
        }
    }
}
