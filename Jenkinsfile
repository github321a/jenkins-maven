pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }
        stage('Test') { 
            steps {
                sh "mvn test site"
            }
            
             post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'   
                }
            }     
        }

        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }


        stage('Build Docker image'){
            steps {
                sh 'docker build -t nhtrbs/docker_jenkins_pipeline:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Push'){
            steps {
                sh 'docker push nhtrbs/docker_jenkins_pipeline:${BUILD_NUMBER}'
            }
        }
        
    }
}
