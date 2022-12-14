def appName='jenkins_test_app'
def changesetNumber='Chset-31'
def snapshotName =null
def changeSetRegResult=''
def changeSetResults=''
pipeline {
   agent any
   stages {
        stage("Clone Repo"){
          steps{
            echo "testing the multibranch configuration in main branch"
          
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
              sleep 10
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
        stage('Register for Prod') {

          when {

            branch "prod-*"

          }

          steps {
            script{
              changeSetResults = snDevOpsConfigGetSnapshots(
                applicationName: "${appName}",
                changesetNumber: "",
                deployableName: "prod",
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
                snapshotName:"${snapshotName}"
                ,showResults:true
              )

              echo"################# Registration result:=${changeSetRegResult}"

            }
          }

        }
        stage('Register for Test') {

          when {

            branch "test-*"

          }

          steps {
            script{
              changeSetResults = snDevOpsConfigGetSnapshots(
                applicationName: "${appName}",
                changesetNumber: "",
                deployableName: "test",
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
                snapshotName:"${snapshotName}"
                ,showResults:true
              )

              echo"################# Registration result:=${changeSetRegResult}"

            }
          }

        }


        stage("Register pipeline using Changeset"){
          steps{
              echo "################# Registering pipeline again using changesetNumber:${changesetNumber}"
              script{
                sleep 5
                changeSetRegResult = snDevOpsConfigRegisterPipeline(
                        applicationName:"${appName}",
                        changesetNumber:"${changesetNumber}"
                        //,snapshotName:"${snapshotName}"
                        ,showResults:true)

                echo "################# Pipeline registration result: ${changeSetRegResult}"  
                
              }
          }
        }
        stage("Initiate ChangeRequest"){
            steps{
                echo "Initating change request for APPLICATION:=${appName} for SNAPSHOT:=${snapshotName}"
                script{
                  if(snapshotName != null){
                    echo"################# Creating CR with AppName:=${appName} and SnapName:=${snapshotName}"
                    changeSetCreateResult = snDevOpsChange(
                      applicationName:"${appName}",
                      snapshotName:"${snapshotName}"
                    )
                  }else{
                    echo"################# Creating generic CR"
                    changeSetCreateResult = snDevOpsChange()
                  }
                    
                    // echo "################# CR creation result:=${changeSetCreateResult}"  
                  
                }
            }
        }
   }
}
