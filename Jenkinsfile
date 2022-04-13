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

        stage('Docker deploy'){
            steps {
                sh 'docker run -itd -p 3000:3000 nhtrbs/docker_jenkins_pipeline:${BUILD_NUMBER}'
            }
        }
        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}
