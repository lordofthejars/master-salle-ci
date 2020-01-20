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
      stage('SonarQube analysis') {
         steps{
         mvn sonar:sonar \
            -Dsonar.projectKey=java \
            -Dsonar.host.url=http://localhost:9000 \
            -Dsonar.login=494216247f2b6884fc6dc61354124cd1a15824e6
         }
      }
      stage('Package') {
         steps {
            echo "Building $version"
            sh script: 'mvn package -DskipTests'
         }
      } 
   }
   post {  
      always {  
         junit '**/nosetests.xml'
         step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
      }  
      success {  
         echo 'This will run only if successful'  
      }  
      failure {  
         mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "byrond27@gmail.com";  
      }  
      unstable {  
         echo 'This will run only if the run was marked as unstable'  
      }  
      changed {  
         echo 'This will run only if the state of the Pipeline has changed'  
         echo 'For example, if the Pipeline was previously failing but is now successful'  
      }  
   }  
}
