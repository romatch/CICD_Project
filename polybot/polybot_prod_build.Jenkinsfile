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
                withCredentials([usernamePassword(credentialsId: 'docker_user_Cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    cd polybot
                    docker login -u $USERNAME -p $PASSWORD
                    docker build -t $DH_NAME/cicd-bolybot:$FULL_VER .
                    docker push $DH_NAME/cicd-bolybot:$FULL_VER
                    '''
                }
            }
        } // Closing brace for "Build" stage

        stage('Trigger Release') {
            steps {
                build job: 'Release', wait: false, parameters: [
                    string(name: 'POLYBOT_PROD_IMG_URL', value: "$DH_NAME/cicd-bolybot:$FULL_VER")
                ]
            }
        }
    }
}
