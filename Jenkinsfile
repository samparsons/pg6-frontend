pipeline {
    agent any 

    triggers {
        pollSCM('* * * * *')
    }
    stages {
        
        stage('NPM install') {
            steps {
                echo '----------------- This is a install phase ----------'
                sh 'npm install'
            }
        }
        
        stage('NPM build') {
            steps {
                echo '----------------- This is a build phase ----------'
                sh 'npm run build'
            }
        }
        
        

        stage('Docker Build') {
            steps {
                echo '----------------- This is a build docker image phase ----------'
                sh '''
                    docker image build -t foodbox-app .
                '''
            }
        }

        stage('Docker Deploy') {
            steps {
                echo '----------------- This is a docker deploment phase ----------'
                sh '''
                 (if  [ $(docker ps -a | grep foodbox-app | cut -d " " -f1) ]; then \
                        echo $(docker rm -f foodbox-app); \
                        echo "---------------- successfully removed ecom-webservice ----------------"
                     else \
                    echo OK; \
                 fi;);
            docker container run --restart always --name foodbox-app -p 4200:4200 -d foodbox-app
            '''
            }
        }
    }
}