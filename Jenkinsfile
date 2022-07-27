def appName='jenkins_test_app'
def changesetNumber='Chset-12'
def snapshotName ='Prod-v2.dpl'
def changeSetRegResult=""
pipeline {
   agent any
   stages {
        stage("Clone Repo"){
          steps{
            echo "testing the multibranch configuration"
          
          }
        }
        stage("Register pipeline using Snapshot"){
          steps{
              echo "Registering pipeline using snapshotName:${snapshotName}"
              script{
                echo "This is inside the register step"
                changeSetRegResult = snDevOpsConfigRegisterPipeline(
                        applicationName:"${appName}",
                        changesetNumber:"${changesetNumber}"
                        snapshotName:"${snapshotName}"
                        ,showResults:true)

                echo "Pipeline registration result: ${changeSetRegResult}"  
                
              }
          }
      }
      stage("Initiate ChangeRequest"){
          steps{
              echo "Initating change request"
              script{
                  changeSetCreateResult = snDevOpsChange(
                      applicationName:"${appName}",
                      snapshotName:"${snapshotName}"
                      )
                  echo ">>>>>>>>>>>${changeSetCreateResult}<<<<<<<<<<<<"  
                
              }
          }
      }
   }
}
