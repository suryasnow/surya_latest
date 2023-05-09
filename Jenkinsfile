pipeline {
   agent any
   stages {
       stage("Build") {
                steps {
                    echo "Building"
                    snDevOpsChange()
                    
                }
       }
        stage("Test") {
           steps {
               echo "Testing"

           }
          
        }
     stage("Deploy") {
             steps{
                  echo ">> Deploy in prod"
                  snDevOpsChange()
              }
      }      
      
  }
}
