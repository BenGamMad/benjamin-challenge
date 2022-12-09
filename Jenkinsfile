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
                sh 'docker image build -t spring-webapp .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'docker image tag spring-webapp bgamboam/spring-webapp:latest'
            }
        }

        
    }
}