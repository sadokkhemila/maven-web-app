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
        stage('upload war to nexus'){
	    steps{
		nexusArtifactUploader artifacts: [	
			[
				artifactId: '01-maven-web-app',
				classifier: '',
				file: 'target/01-maven-web-app-3.0.war',
				type: war		
			]	
		],
		credentialsId: 'nexus-cred',
		groupId: 'in.ashokit',
		nexusUrl: '192.168.34.100:8081',
		protocol: 'http',
		repository: 'maven-nexus-repo'
		version: '3.0'
	        }
            }
        
        }
}
    
