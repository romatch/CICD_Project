pipeline {
    agent any
    options {
        timestamps()
    }

    environment {
        DH_NAME = "romkatch"
        FULL_VER = "0.0.$BUILD_NUMBER"
    }
    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_user_Cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                {
                    sh '''
                    docker login -u $USERNAME -p $PASSWORD
                    docker build -t $DH_NAME/cicd-Bolybot:$FULL_VER .
                    docker push $DH_NAME/cicd-Bolybot:$FULL_VER
                    '''
                }

            }
}

