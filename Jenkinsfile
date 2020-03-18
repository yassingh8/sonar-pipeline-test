pipeline {
  agent any
  stages {
	stage('Unit Test') {
	   steps {
	       sh(label: 'Test running', script: '''mvn test'''
	       echo 'Hello Testing done')
       }
   	}
	stage('Jacoco Coverage Report') {
	   steps{
		jacoco()
		}
	}	
 	/* stage('SonarQube'){
       steps{
           sh(label: '', script: '''mvn sonar:sonar \
		 -Dsonar.host.url=http://localhost:9000 \
 		-Dsonar.login=3baa9598f75efbfdf94fd6e17664d4213b73e7c0''')
       }
   }*/
	stage('Maven Build'){
		steps{
				sh(label:'Maven Build of war file', script:'''
					mvn clean install -DskipTests=false
					mvn package
				''')
		}
	}
	 stage('Images'){
       steps{
           sh("docker build -t ayushmalviya27/app1 .")
       }
   }

 

    stage('Image push'){
       steps{
           sh("docker push ayushmalviya27/app1")
       }
   }

 

    stage('Image pull'){
       steps{
           sh("docker pull ayushmalviya27/app1")
       }
   }    
    
  }
}