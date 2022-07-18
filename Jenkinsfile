pipeline {

    agent {
        node {
            label 'master'
        }
    }

    //Define Fixed Parameter
    parameters {
        choice(name: 'ref', choices:['integration'], description: 'Github branch for deployment')
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '10', 
                    numToKeepStr: '3'
            )
    }

    stages {
           
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                echo "Cleaned Up Workspace For Project"
            }
        }
        
        stage('Check Environment And PR Status') {
            steps{
                script{
                    if(params.current_status == "closed" && params.merged == true){
                        if(params.ref == 'integration') {
                            build job: 'Integration_Deployment', 
                            parameters: [
                                string(name: 'BRANCH_NAME', value: params.ref)
                            ]
                        }else{
                            echo "Magennto Cloud branch were not found in the merge request"
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
