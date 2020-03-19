pipeline {
//    {dockerfile true} 
  agent any
  tools {
    maven 'maven3';
}
 
  stages {
    stage('Unit Test') {
       steps {
           sh label: 'Test running', script: '''mvn test'''
           echo 'Hello Testing done'
       }
       }
    stage('Jacoco Coverage Report') {
       steps{
        jacoco()
        }
    }   
   //   stage('SonarQube'){
   //     steps{
   //         sh label: '', script: '''mvn sonar:sonar \
   //       -Dsonar.host.url=http://10.0.1.159:9000 \
   //       -Dsonar.login=228f84cc5b4a76fdf334257472ce2cd2c9b947ed'''
   //     }
   // }
    stage('Maven Build'){
        steps{
                sh label:'Maven Build of war file', script:'''
                    mvn clean install -DskipTests=false
                    mvn package
                '''
        }
    }
  stage('Docker Image Build') {
      steps{
         sh "docker build -t my_hello_app:latest ."
      }
    }
    stage('Docker save'){
        steps{
            sh "docker save my_hello_app:latest>my_hello_app.tar"
        }
    }
 
    stage('Upload to S3'){
        steps{
            withAWS(region:'us-east-1',credentials:'aws-cred')
            {
                s3Upload(bucket:'bucketforsprint',file:'my_hello_app.tar',workingDir:'./');
            }
        }
    }
    stage('Tar remove'){
        steps{
            sh "rm my_hello_app.tar"
        }
    }
  }
 
}
