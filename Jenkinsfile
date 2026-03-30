pipeline {
    agent { label 'deploy' }

stages {

    stage('Code') {
        steps {
            dir('git_repos') {
                git branch: 'jenkins', url: 'https://github.com/ashok280705/chatify'
            }
            echo "done with code"
        }
    }

    stage('Create ENV') {
        steps {
            dir('git_repos') {
                sh '''
                echo "PORT=3000" > .env
                echo "MONGODB_URI=mongodb://mongodb:27017/chatify" >> .env
                echo "NEXTAUTH_URL=http://13.201.12.80.nip.io:3000" >> .env
                '''
            }
        }
    }

    stage('Build') {
        steps {
            dir('git_repos') {
                sh 'docker build -t chatify .'
            }
        }
    }

    stage('Push to DockerHub') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: 'dockerhub',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {
                sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker tag chatify $DOCKER_USER/chatify:latest
                docker push $DOCKER_USER/chatify:latest
                '''
            }
        }
    }

    stage('Deploy') {
        steps {
            dir('git_repos') {
                sh '''
                docker-compose down || true
                docker-compose up -d --build
                '''
            }
        }
    }

}
}
