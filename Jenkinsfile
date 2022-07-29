def appName='jenkins_test_app'
def changesetNumber='Chset-12'
def snapshotName =''
def changeSetRegResult=''
def changeSetResults=''
pipeline {
   agent any
   stages {
        stage("Clone Repo"){
          steps{
            echo "testing the multibranch configuration"
          
          }
        }
        stage('Branch-Fix* Steps') {

          when {

            branch "fix-*"

          }

          steps {
              echo "this is from branch fix-*"

          }

        }
        stage('Register for Preprod') {

          when {

            branch "preprod-*"

          }

          steps {
            script{
              changeSetResults = snDevOpsConfigGetSnapshots(
                applicationName: "${appName}",
                changesetNumber: "",
                deployableName: "preprod",
                outputFormat:"xml",
                isValidated:false,
                showResults:false,
                markFailed:false
              )
              
              echo"################# ChangeSetResult:=\n${changeSetResults}"

              if (changeSetResults !=null){
                def changeSetResultsObject = readJSON text: changeSetResults
                changeSetResultsObject.each {
                  snapshotName = it.name
                }
              }

              echo"################# Snapshot going to be registered:= ${snapshotName}"
            
              changeSetRegResult = snDevOpsConfigRegisterPipeline(
                applicationName:"${appName}",
                // changesetNumber:"${changesetNumber}"
                snapshotName:"${snapshotName}"
                ,showResults:true
              )

              echo"################# Registration result:=${changeSetRegResult}"

            }
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
