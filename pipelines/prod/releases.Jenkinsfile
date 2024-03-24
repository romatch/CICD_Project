pipeline {
    agent any

    parameters { string(name: 'IMG_URL', defaultValue: '', description: '') }

    stages {
        stage('Update YAML') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {

                    sh '''
                   echo "IMG_URL: ${IMG_URL}"
                    case "${IMG_URL}" in
                        *-polybot*) YAML_FILE="k8s/prod/polybot.yaml";;
                        *-yolo5*) YAML_FILE="k8s/prod/yolo5.yaml";;
                        *)
                            echo "Unknown IMG_URL: ${IMG_URL}"
                            exit 7;;
                    esac

                    git checkout releases
                    git merge origin/main
                    sed -i "s|image: .*|image: ${IMG_URL}|g" $YAML_FILE
                    git add $YAML_FILE
                    git commit -m "$IMG_URL"
                    git push https://romatch:$PASSWORD@github.com/romatch/CICD_Project.git releases

                    '''
                }
            }
        }
    }
}