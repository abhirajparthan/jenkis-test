// =============================================================================================================================
// TITLE:            Jenkinsfile
// DESCRIPTION:      Script for Manage the Deployment Grafana with Terraform IaaC Scripts using Jenkins CI/CD
// AUTHOR:           Abhiraj Parthan
// DATE:             2023-06-27
// VERSION:          0.1
// =============================================================================================================================

// =============================================================================================================================
// Jenkins Parameter Definition
// =============================================================================================================================
properties([
    parameters([
        [$class: 'ChoiceParameter', 
            choiceType: 'PT_SINGLE_SELECT', 
            description: 'Select the Target or Flavors from the Dropdown List',  
            name: 'Targets',  
            script: [
                $class: 'GroovyScript', 
                fallbackScript: [
                    classpath: [], 
                    sandbox: false, 
                    script: 
                    'return[\'Could not get Targets or Flavors\']'
                ], 
                script: [
                    classpath: [], 
                    sandbox: false, 
                    script: 
                    'return["pgat", "nba"]'
                ]
            ]
        ],
        [$class: 'ChoiceParameter', 
            choiceType: 'PT_SINGLE_SELECT', 
            description: 'Select the  Environment from the Dropdown List',  
            name: 'Moniter',  
            script: [
                $class: 'GroovyScript', 
                fallbackScript: [
                    classpath: [], 
                    sandbox: false, 
                    script: 
                    'return[\'Could not get Moniter\']'
                ], 
                script: [
                    classpath: [], 
                    sandbox: false, 
                    script: 
                    'return["registration","sportsdata"]'
                ]
            ]
        ], 
        [$class: 'ChoiceParameter', 
            choiceType: 'PT_SINGLE_SELECT', 
            description: 'Select the  Environment from the Dropdown List',  
            name: 'Environments',  
            script: [
                $class: 'GroovyScript', 
                fallbackScript: [
                    classpath: [], 
                    sandbox: false, 
                    script: 
                    'return[\'Could not get Environments\']'
                ], 
                script: [
                    classpath: [], 
                    sandbox: false, 
                    script: 
                    'return["dev","stage","prod"]'
                ]
            ]
        ], 

    ])
])// End of Parameter Definitions


// =============================================================================================================================

// Begin the Pipeline Script
// =============================================================================================================================

pipeline {
    agent any
    options {
        timeout(time: 4, unit: 'HOURS')
    }

    // =====================================================================================================================        
    //  Environment Variables 
    // =====================================================================================================================        
    environment {
    client_name = "${params.Targets == "pgat" ? "pga" : "nba"}"
    //directory = "${WORKSPACE}/dashboard/${client_name}/"
    directory ="${WORKSPACE}/grafana/${client_name}"
    dash = "${params.Moniter == "registration" ? "regmon" : "sdmon"}"
    file = "${params.Moniter == "registration" ?  "pga_regmon.txt" : "pga_sdmon.txt"}"
    //TF_VAR_URL = "${params.Environments == "prod" ? "https://${params.Targets}.grafana.quintar.ai" : "https://${params.Targets}-${params.Environmnets}.grafana.quintar.ai"}"
    TF_VAR_AUTH = "${params.Targets}-${params.Environments}-grafana"
    pga_regmon = "${WORKSPACE}/grafana/pga/regmon"
    pga_sdmon = "${WORKSPACE}/grafana/pga/sdmon"
    }

// =========================================================================================================================
    stages {
        stage ("Build the Grafana DashBoard") {
            steps {
                script {
                    // withCredentials([gitUsernamePassword(credentialsId: 'e57b9076-77c3-417f-b36b-50e413520223', gitToolName: 'Default')]) {
                        wrap([$class: 'BuildUser']) {
                            if ((params.Targets.equals("pgat"))) {
                                    sh """
                                    echo "dev"
                                    cd ${directory}/${dash}
                                    cat ${file}
                                    #terraform init
                                    #terraform validate 
                                    #terraform plan
                                    #terraform apply -auto-approve

                                    """
                                }
                                    
                            }     
                    // }
                }
            }
        } // End of stage Build the Android Apps
        // ---------------------------------------------------------------------------------------------------------------
    } // End of Stages
// =====================================================================================================================        
//  Jenkins Notification
// =====================================================================================================================
//  post {
//         always {
//            script{
//               wrap([$class: 'BuildUser']) { 
//                env.GIT_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
//                env.GIT_BRANCH = env.GIT_URL.replaceFirst(/^.*\/([^\/]+?).git$/, '$1')
//                def buildStatus = currentBuild.currentResult
//                if (buildStatus == 'SUCCESS') {
//                     slackSend channel:'#jenkins',
//                          color: color_slack_msg(), 
//                                message: "NReal Spatial build is successfully generated and uploaded to Azure Blob Storage <${Blob_url}|Click Here to Download & Install>.\n *Build Status:*\n NReal Spatial build is ${currentBuild.currentResult}.\n *Triggered By:*\n ${env.BUILD_USER}\n *Environment:*\n ${Environments}\n *Build Number:*\n ${env.BUILD_NUMBER}\n *Image Version:*\n *Git Branch:*\n ${env.GIT_BRANCH}\n *Git Commit message :*\n ${env.GIT_COMMIT_MSG}\n *Result:*\n ${currentBuild.currentResult}\n *More info:* ${env.BUILD_URL}"
//                } else {
//                     slackSend channel:'#jenkins',
//                         color: color_slack_msg(), 
//                                message: "NReal Spatial build is ${currentBuild.currentResult}.\n *Triggered By:*\n ${env.BUILD_USER}\n *Environment:*\n ${Environments}\n *Build Number:*\n ${env.BUILD_NUMBER}\n *Image Version:*\n *Git Branch:*\n ${env.GIT_BRANCH}\n *Git Commit message :*\n ${env.GIT_COMMIT_MSG}\n *Result:*\n ${currentBuild.currentResult}\n *More info:* ${env.BUILD_URL}"
//                }
//                }    
//             }
//         }
//     }       
}  // End of Pipeline
def color_slack_msg() {
    switch(currentBuild.currentResult) {
    case "SUCCESS":
        return "good"
        break
    case "FAILURE":
    case "UNSTABLE":
        return "danger"
        break
    }
}
