def appName='jenkins_test_app'
def changesetNumber='Chset-65'
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
      stage('cat README') {

     when {

       branch "fix-*"

     }

     steps {
        echo "this is from branch fix-*"

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
                        //,snapshotName:"${snapshotName}"
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
