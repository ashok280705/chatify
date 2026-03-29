pipeline {
    agent any

    stages {

        stage('Build + Push + Deploy on EC2-2') {
            steps {
                sshagent(['deploy-server']) {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.7.122.54 "

                      
                        mkdir -p /home/ubuntu/git_repos &&
                        cd /home/ubuntu/git_repos &&

                       
                        if [ ! -d chatify ]; then
                            git clone https://github.com/ashok280705/chatify.git
                        else
                            cd chatify && git pull origin jenkins
                        fi &&

                        cd /home/ubuntu/git_repos/chatify &&

                        docker build -t chatify . &&

               
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin &&

                  
                        docker tag chatify $DOCKER_USER/chatify:latest &&
                        docker push $DOCKER_USER/chatify:latest &&

                   
                        docker-compose down &&
                        docker-compose up -d --build

                        "
                        '''
                    }
                }
            }
        }

    }
}
