def changeRequestNumber = "null"
def stageName = "STAGE"
def ciPipelineName = "CI_Pipeline"
def currStageName = "none"
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script{
                    currStageName = "Build"
                }
                echo '${currStageName} - START'
                echo '${currStageName} - END'                
            }
        }
        stage('Create_Change') {
            steps {
                script{
                    currStageName = "Create_Change"
                }
                echo "${currStageName} Step - START"
                echo "changeRequestNumber before snDevOpsChange() - ${changeRequestNumber}, stageName - ${currStageName}"
                snDevOpsStep()
                snDevOpsChange()
                echo "${currStageName} Step - END"
                echo "changeRequestNumber after snDevOpsChange() but unassigned to snDevOpsGetChangeNumber() - ${changeRequestNumber}, stageName - ${currStageName}"
            }
        }

        stage('Get_Change') {
            steps {
                script{
                    echo "Pipeline Name, Build Number, Stage Name are mandatory input parameters to retrieve the change request number. If these input field parameters are not provided, the change request number for the current Pipeline Name, Build Number, Stage Name will be retrieved. For multi-branch pipelines, you must provide the Branch Name as an input parameter as well. (starting with version 1.37)."
                    echo "*****"
                    echo "snDevOpsGetChangeNumber with only stage_name as input parameter"
                    changeRequestNumber = snDevOpsGetChangeNumber(changeDetails: """{"stage_name":"${currStageName}"}""")
                    echo "changeRequestNumber after snDevOpsGetChangeNumber() step with stage_name only parameter is - ${changeRequestNumber}"
                }
            }
        }

        stage('Update_Change') {
            steps {
                script{
                    currStageName = "Update_Change"
                    echo "Updating the changeDetails for changeRequestNumber ${changeRequestNumber}"
                    snDevOpsUpdateChangeInfo(changeRequestDetails: 
                        """{
                            "short_description": "Short description updated to Test description in ${currStageName} Step by ${env.BUILD_NUMBER}",
                            "priority": "1",
                            "start_date": "2021-02-05 08:00:00",
                            "end_date": "2022-12-25 08:00:00",
                            "justification": "test justification",
                            "description": "test description",
                            "cab_required": true,
                            "comments": "This update for comments is from jenkins file",
                            "work_notes": "This update for work_notes is from jenkins file"
                        }""", 
                        changeRequestNumber: """${changeRequestNumber}""")
                }
            }
        }
    }
}
