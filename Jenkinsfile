pipeline {

    agent {
        node {
            label 'master'
        }
    }

    //Define Fixed Parameter
    //parameters {
   //     choiceee(name: 'ref', choices:['integration'], description: 'Github branch for deployment')
    //}

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '10', 
                    numToKeepStr: '3'
            )
    }

    stages {
           
        stage('Cleanup Workspacee') {
            steps {
                cleanWs()
                echo "Cleaneed Up Workspace For Project"
            }
        }
        
        stage('Check Environment And PR Status') {
            steps{
                script{
                    if(params.current_status == "closed" && params.merged == true){
                        if(params.ref == 'integration') {
                            echo "Requested PR is merged"
                            //build job: 'Integration_Deployment', 
                            //parameters: [
                                //string(name: 'BRANCH_NAME', value: params.ref)
                            //]
                        }else{
                            echo "Requested PR is not merged now"
                            currentBuild.result = 'FAILURE'
                            return
                        }
                    }else{
                        echo "Requested PR is not merged"
                        currentBuild.result = 'FAILURE'
                        return
                    }
                }
            }
        }

    }
}
