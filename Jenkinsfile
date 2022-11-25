pipeline{
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage("Checkout") {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mohdfaaiz/Jenkins-Docker']]])
            }
        }  
        
        
       stage('Build Maven'){
            steps{
                sh 'mvn clean install -DskipTests'
            }
         }
        
        
       stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('Sonarqube') {
                    // Optionally use a Maven environment you've configured already
                   
                        sh 'mvn sonar:sonar'
                    
                }
            }
        }
       stage("Build Docker Image") {
            steps {
                script {
                    sh 'docker build -t myapp .'
                    sh 'docker tag myapp mohammedfaaiz/myapp:latest'
                    sh 'mvn -version'
                }
            }
        }
        
        stage('pushing to dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerhubpwd')]) {
                  // some block
                    sh 'docker login -u mohammedfaaiz -p ${dockerhubpwd}'
                    
                    sh 'docker push mohammedfaaiz/myapp:latest'
                    }
                }
            }
        }
        

        



       
    }
}
