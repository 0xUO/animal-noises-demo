pipeline {
    agent any
    stages {
        stage('test') {
            steps {               
                    sh "bash test.sh"
            }
        }
        stage('build and push') {
            environment {
                DOCKER_CREDS = credentials('docker-creds')
            }
            steps {             
                sh "docker-compose build --parallel"
                sh "docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW}"
                sh "docker-compose push"
                sh "/bin/bash -c 'docker rmi -f \$(docker images -q)'"  
            }
        }
        stage('deploy swarm') {
            steps {
                sh "echo '    driver: overlay' >> docker-compose.yaml"
                sh "scp ./docker-compose.yaml jenkins@swarm-manager:/home/jenkins/docker-compose.yaml"
                sh "scp ./nginx.conf jenkins@swarm-manager:/home/jenkins/nginx.conf"
                sh "ssh jenkins@swarm-manager < deploy.sh"
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: "*/htmlcov/*"
        }
    }
}