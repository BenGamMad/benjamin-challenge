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
    }
}