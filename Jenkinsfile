def appName='jenkins_test_app'
def changesetNumber='Chset-33'
def snapshotName =''
def changeSetRegResult=''
def changeSetResults=''
pipeline {
   agent any
   stages {
        stage("Clone Repo"){
          steps{
            echo "testing the multibranch configuration in preprod branch"
          
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
            git branch: 'preprod-1', url: 'https://github.com/iamkumaramit/multibranch_register.git'
            // the e2e sceanrio for the test branch
            script{
              // Uploading the config file
                uploadResults = snDevOpsConfigUpload(
                         applicationName: "${appName}",
                         deployableName: "preprod",
                         changesetNumber:"",
                         dataFormat: "json",
                         configFile: "configPreprod.json",
                         target:"deployable",
                         namePath: "/configUpload/${currentBuild.number}",
                         autoCommit:true,
                         autoValidate:true,
                         convertPath:false,
                         showResults:true,
                         markFailed:false
                 )
                echo "################# uploadResults:= ${uploadResults}"
              
              //Getting the snapshot details
              sleep 10
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
              sleep 5
              changeSetRegResult = snDevOpsConfigRegisterPipeline(
                applicationName:"${appName}",
                snapshotName:"${snapshotName}"
                ,showResults:true
              )

              echo"################# Registration result:=${changeSetRegResult}"

              //publishing the snapshot 
              sleep 5
              publishResult = snDevOpsConfigPublish(
                    applicationName:"${appName}",
                    deployableName:"preprod",
                    snapshotName: "${snapshotName}",
                    showResults:true,
                    markFailed:false
                )
              echo"################# Publish result:= ${publishResult}"

              //Exporting the snapshot
              sleep 5
              exportResult = snDevOpsConfigExport(
              applicationName: "${appName}", 
              snapshotName: "", 
              deployableName: "preprod", 
              exporterFormat: "json", 
              fileName:"${appName}-preprod-${currentBuild.number}.json", 
              exporterName: "returnAllData-now",
              showResults:true,
              markFailed:false
              )

              echo"################# Export result:= ${exportResult}"

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
                    // changeSetCreateResult = snDevOpsChange(
                    //   applicationName:"${appName}",
                    //   snapshotName:"${snapshotName}"
                    // )
                  }else{
                    echo"################# Creating generic CR"
                    // changeSetCreateResult = snDevOpsChange()
                  }
                    
                    // echo "################# CR creation result:=${changeSetCreateResult}"  
                  
                }
            }
        }
   }
}
