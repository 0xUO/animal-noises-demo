pipelike {
    agent any
    stages {
        stage("test") {
            steps {
                dir('animal-noises-demo')
                sh "bash test.sh"
            }

        }
    }
    post {
        always{
            archiveArtifacts artifacts: "animal-noises/htmlcov/*"
        }
    }

}