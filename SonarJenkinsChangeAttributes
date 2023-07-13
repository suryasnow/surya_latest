def artifactname = "artifact_devops_${env.BUILD_NUMBER}.jar"
def repoName = "repository_devops"
def pipelineName = "pipeline_devops"
def semanticVersion = "${env.BUILD_NUMBER}.0.0"
def packageName = "package_devops_${env.BUILD_NUMBER}"
def version = "${env.BUILD_NUMBER}.0"
def changeRequestId = "defaultChangeRequestId"

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
	      sleep(5);
           }
          post {
             always {
                junit "**/target/surefire-reports/*.xml"
             }
           }
	  
       }
       
       stage('Pre-Prod') {
		steps {
		    sleep(5);
                    //sonarSummaries()
                    snDevOpsArtifact(artifactsPayload: """{"artifacts": [{"name": "${artifactname}", "version": "1.${env.BUILD_NUMBER}","semanticVersion": "1.${env.BUILD_NUMBER}.0","repositoryName": "${repoName}"}],"branchName":"main"}""")
                    snDevOpsPackage(name: "${packageName}", artifactsPayload: """{"artifacts":[{"name": "${artifactname}", "version": "1.${env.BUILD_NUMBER}","semanticVersion": "1.${env.BUILD_NUMBER}.0","repositoryName": "${repoName}"}], "branchName":"main"}""")
            }       
       }
      stage('Deploy') {
                 steps {
		    
                    echo 'Deploying the change....'
                    //snDevOpsChange(ignoreErrors:false)
                    snDevOpsChange(ignoreErrors:false,changeRequestDetails:{attributes:{short_description:Test description,priority:1,start_date:2021-02-05 08:00:00,end_date:2022-04-05 08:00:00,justification:test justification,description:test description,cab_required:true,comments:This update for work notes is from jenkins file,work_notes:test work notes,assignment_group:a715cd759f2002002920bde8132e7018},setCloseCode:false})
		    script {
		    	echo 'Inside script step...'
		    	changeRequestId = snDevOpsGetChangeNumber()
			//changeRequestId = snDevOpsGetChangeNumber(changeDetails: """{"build_number":"${env.BUILD_NUMBER}","pipeline_name":pipelineName,"stage_name":"${stageName}"}""")
			echo "Change Request Id without any attributes... ${changeRequestId}"
		    }
		    
                 }
      }

 }
 
}

def sonarSummaries() {

   //withSonarQubeEnv('Sonar_Cloud') {
       //sh '${SCANNER_HOME}/bin/sonar-scanner -Dproject.settings=${SCANNER_HOME}/conf/qa-sonar-scanner-cloud.properties'
	 //}

    withSonarQubeEnv('sonarQube_local'){
      if(fileExists("sonar-project.properties")) {
      	sh '${SCANNER_HOME}/bin/sonar-scanner'
        } else {
            sh '${SCANNER_HOME}/bin/sonar-scanner -Dproject.settings=${SCANNER_HOME}/conf/qa-sonar-scanner.properties'
   		  //sh '${SCANNER_HOME}/bin/sonar-scanner -Dproject.settings=${SCANNER_HOME}/conf/sonar-scanner.properties'
        }
	  }
   // timeout(time: 1, unit: 'MINUTES') {
   //     waitForQualityGate abortPipeline: false
   // }
} // end of def sonarsummaries
