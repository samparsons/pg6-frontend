node(){
    stage('Cloning Git') {
        checkout scm
    } 
    stage('Install dependencies') {
        nodejs('nodejs') {
            sh 'npm install'
            echo "Modules installed"
        }
    }

    stage('Build') {
        nodejs('nodejs') {
            sh 'npm run build'
            echo "Build completed"
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
        docker container run --restart always --name foodbox-service-rest -p 4200:4200 -d foodbox-app
        '''
        }
    }
}