pipeline{
    agent{
        label 'qa-worker'
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

        stage('Unit test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Building docker image'){
            steps {
                sh 'sudo docker image build -t spring-webapp .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'sudo docker image tag spring-webapp bgamboam/spring-webapp:latest'
            }
        }

        stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'docker-credentials')]) {
                    sh 'docker login -u bgamboam -p ${dockerpwd}'
                    sh 'docker image push bgamboam/spring-webapp:latest'
                }
            }
        }
        
    }
}