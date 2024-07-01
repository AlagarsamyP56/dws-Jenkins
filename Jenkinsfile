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
                    if (isUnix()) {
                        sh 'mvn clean deploy -DmuleDeploy -Dusername=${ANYPOINT_CREDENTIALS_USR} -Dpassword=${ANYPOINT_CREDENTIALS_PSW} -DworkerType=Micro -Dworkers=1 -Denv=dev -Dsecure.key=******'
                    } else {
                        bat 'mvn clean deploy -DmuleDeploy -Dusername=${ANYPOINT_CREDENTIALS_USR} -Dpassword=${ANYPOINT_CREDENTIALS_PSW} -DworkerType=Micro -Dworkers=1 -Denv=dev -Dsecure.key=******'
                    }
                }
            }
        }
    }
}
