pipeline {
   agent any
   stages {
      stage("Git Checkout") {
         steps {
            echo "Git checkout"
            git "https://github.com/tambuzi/master-salle-ci"
         }
      }
      stage("Cleaning Package and Compile") {
         steps {
            echo “Cleaning Package”
            sh script: "./mvnw clean package -DskipTests"
            echo "Compile the package"
            sh label:"", script: "./mvnw compile"
         }
      }
      stage ("Tests") {
          steps {
            sh script: "./mvnw test"
            junit "target/surefire-reports/*.xml"
          }
      }
      stage("Acceptance Test"){
         steps{
            echo "Executing Acceptance Test"
            sh script: "./mvnw verify"
         }
      }
      stage("Package") {
         steps {
            echo “Package”
            sh script: "./mvnw package -DskipTests"
         }
      }
     stage("Build Docker Image") {
         steps {
            sh "docker rm -f maven || (exit 0)"
            sh "docker build -f src/main/docker/Dockerfile.jvm -t quarkus/code-with-quarkus-jvm ."
         }
     }
     stage("Run Container on Server") {
        steps {
            sh "docker run --name maven -i --rm -p 8081:8080 -d quarkus/code-with-quarkus-jvm"
            //echo "Executing Tests"
            //sh script: "./mvnw test"
            //junit "target/surefire-reports/*.xml"
            //echo "Executing Acceptance Tests"
            //sh script: "./mvnw -Dtest=ExampleResourceIT test"
            //junit "target/surefire-reports/*.xml"
        }
     }
     stage("Produccion") {
        input {
            message “Pasamos a producción?”
        }
        steps {
           echo "Production"
            sh "docker rm -f maven || (exit 0)"
            sh "docker build -f src/main/docker/Dockerfile.jvm -t quarkus/code-with-quarkus-jvm ."
            sh "docker run --name maven -i --rm -p 8082:8080 -d quarkus/code-with-quarkus-jvm"
        }
     }
   }
   }
   post {  
      always {  
         junit "**/nosetests.xml"
         step([$class: "CoberturaPublisher", autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: "**/coverage.xml", failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: "ASCII", zoomCoverageChart: false])
      }  
      success {  
         echo "This will run only if successful"  
      }  
      failure {  
         mail bcc: "", body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: "", charset: "UTF-8", from: "", mimeType: "text/html", replyTo: "", subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "byrond27@gmail.com";  
      }  
      unstable {  
         echo "This will run only if the run was marked as unstable"  
      }  
      changed {  
         echo "This will run only if the state of the Pipeline has changed"  
         echo "For example, if the Pipeline was previously failing but is now successful"  
      }  
   }  
}
