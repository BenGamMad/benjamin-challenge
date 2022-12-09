pipeline{
    agent{
        label 'dev-worker'
    }

    stages{
        stage('Preparing stage'){
            steps{
                sh 'echo "Preparing the stage for the application"'
            }
        }
        stage('Compilation'){
            steps{
                sh 'mvn -DskipTests clean install package'
            }
        }
        stage('Building docker image'){
            steps {
                sh 'docker image build -t spring-webapp .'
            }
        }
        stage('Tag docker image') {
            steps {
                sh 'docker image tag spring-webapp bgamboam/spring-webapp:latest'
            }
        }
         stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'dockerpwd-id', variable: 'dockerpwd')]) {
                    sh 'docker login -u bgamboam -p ${dockerpwd}'
                    sh 'docker image push bgamboam/spring-webapp:latest'
                }
            }
        }
    }
}