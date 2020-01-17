pipeline {
   agent any
    
       stages {
          stage('Compile & Test') {
             steps {
                    echo 'Hello From Jenkinsfile'
                    git 'https://github.com/lordofthejars/master-salle-ci.git'
                    sh label: '', script: 'mvn compile test'
                    junit 'target/surefire-reports/*.xml'
             }
          }
          stage('Package') {
              steps {
                echo "Building $version"
                sh script: 'mvn package -DskipTests'
              }
          }
     
   }
}
