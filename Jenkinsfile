pipeline {
  agent any
  stages {
	stage('Unit Test') {
	   steps {
	       bat label: 'Test running', script: '''mvn test'''
	       echo 'Hello Testing done'
       }
   	}
	stage('Jacoco Coverage Report') {
	   steps{
		jacoco()
		}
	}	
 	stage('SonarQube'){
       steps{
           bat label: '', script: '''mvn sonar:sonar \
		 -Dsonar.host.url=http://localhost:9000 \
 		-Dsonar.login=3baa9598f75efbfdf94fd6e17664d4213b73e7c0'''
       }
   }
	stage('Maven Build'){
		steps{
				bat label:'Maven Build of war file', script:'''
					mvn clean install -DskipTests=false
					mvn package
				'''
		}
	}
	 stage('Images'){
       steps{
           bat "docker build -t ayushmalviya27/app1 ."
       }
   }

 

    stage('Image push'){
       steps{
           bat "docker push ayushmalviya27/app1"
       }
   }

 

    stage('Image pull'){
       steps{
           bat "docker pull ayushmalviya27/app1"
       }
   }    
    
  }
}