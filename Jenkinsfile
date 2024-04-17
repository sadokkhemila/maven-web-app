pipeline {
    agent any
    tools {
        maven "maven"
    }
   
    
    stages {
         stage("Clone code from VCS") {
             steps {
                script {
                    git 'https://github.com/sadokkhemila/maven-web-app.git';
                }
            }
        }
         stage("Maven Build") {
             steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
	stage('Build Docker Imager et container'){
	   steps {
                script {
		      
                      sh 'docker build -t sadok/myweb:0.0.1 .'
		      sh 'docker run -d -p 8085:8085 --name myweb sadok/myweb:0.0.1'
		      
                }
            }
            
        }
	 stage('test sonarqube'){
            steps{
                withSonarQubeEnv('sonarQube-server') {
                    sh 'mvn clean package sonar:sonar '
                }
            }
        }
         stage('upload war to nexus'){
	     steps{
		
		  nexusArtifactUploader artifacts: [	
			         [
				      artifactId: '01-maven-web-app',
				      classifier: '',
				      file: 'target/01-maven-web-app.war',
				      type: 'war'	
			        ]	
		  ],
		  credentialsId: 'nexus-cred',
		  groupId: 'in.ashokit',
		  nexusUrl: '51.38.50.55:8081',
		  nexusVersion: 'nexus3',
		  protocol: 'http',
		  repository: 'testmaven',
		  version: '3.0-SNAPSHOT'
	        }
            }
	 
     }
}
    
