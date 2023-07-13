def artifactname = "artifact_devops_${env.BUILD_NUMBER}.jar"
def repoName = "repository_devops"
def pipelineName = "pipeline_devops"
def semanticVersion = "${env.BUILD_NUMBER}.0.0"
def packageName = "package_devops_${env.BUILD_NUMBER}"
def version = "${env.BUILD_NUMBER}.0"

pipeline {
  agent any
  tools {
       maven 'Maven'
   }
   environment {
	 SCANNER_HOME = tool 'sonarScanner'
	}  
  stages {
       stage('Build') {
           steps {
              sh 'mvn -B -DskipTests clean compile'
           }
       }
       stage('Test') {
           steps {
              sh 'mvn test'
           }
           post {
             always {
                junit "**/target/surefire-reports/*.xml"
             }
           }
       }
       
       stage('Pre-Prod') {
		steps {
                    //sonarSummaries()
		    script{
		       sh '${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=sunilyerubandi_DemoMavenProject -Dsonar.organization=sunilyerubandi -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=8f73ecadbe478e324214c741569315dddfa01ee5 -Dsonar.java.binaries=target/ -Dsonar.branch.name=master'
		    }
                    snDevOpsArtifact(artifactsPayload: """{"artifacts": [{"name": "${artifactname}", "version": "1.${env.BUILD_NUMBER}","semanticVersion": "1.${env.BUILD_NUMBER}.0","repositoryName": "${repoName}"}],"branchName":"main"}""")
                    snDevOpsPackage(name: "${packageName}", artifactsPayload: """{"artifacts":[{"name": "${artifactname}", "version": "1.${env.BUILD_NUMBER}","semanticVersion": "1.${env.BUILD_NUMBER}.0","repositoryName": "${repoName}"}], "branchName":"main"}""")
            }       
       }
      stage('Deploy') {
                 steps {
                    echo 'Deploying the change....'
                    snDevOpsChange(ignoreErrors:false)
                 }
      }

 }
 
}

def sonarSummaries() {

   //withSonarQubeEnv('Sonar_Cloud') {
       //sh '${SCANNER_HOME}/bin/sonar-scanner -Dproject.settings=${SCANNER_HOME}/conf/qa-sonar-scanner-cloud.properties'
       
	//}
	
    //withSonarQubeEnv('sonarQube_local') {
      // 	sh '${SCANNER_HOME}/bin/sonar-scanner -Dproject.settings=${SCANNER_HOME}/conf/qa-sonar-scanner.properties'
   		// sh '${SCANNER_HOME}/bin/sonar-scanner -Dproject.settings=${SCANNER_HOME}/conf/sonar-scanner.properties'
	//}
	
} // end of def sonarsummaries
