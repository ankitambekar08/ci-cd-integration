pipeline {

    agent {
        node {
            label 'master'
        }
    }

    //Define Fixed Parameter
    parameters {
        choice(name: 'ref', choices:['development'], description: 'Github branch for deployment')
    }

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
                    if(params.pull_request_status == "closed" && params.merged_status == true){
                        if(params.ref == 'development') {
                            echo "Reqfuested PR is merged"
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
