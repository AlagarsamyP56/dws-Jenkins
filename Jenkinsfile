pipeline{

agent any

stages {

stage('Build Application') {

steps {

sh 'mvn clean install'

}
}
stage('Deploy CloudHubs') {

environment {

ANYPOINT_CREDENTIALS = credentials('anypoint.alagar')

}

steps {

echo 'Deploying mule project due to the latest code commit…'

echo 'Deploying to the configured environment….'

sh 'mvn clean deploy -DmuleDeploy  -Dusername=${ANYPOINT_CREDENTIALS_USR} -Dpassword=${ANYPOINT_CREDENTIALS_PSW} -DworkerType=Micro -Dworkers=1 -Denv=dev -Dsecure.key=******'

}

}
}
}