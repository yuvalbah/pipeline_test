#!/usr/bin/env groovy
// =============================================== Var zone ===============================================
SETUPS = [:]

// ========================================= Classes and methods declaration =====================================

// Define variables
String providers = "aws,gcp,azure"
String build_versions = "14 or earlier,15 and above"
String gw_modes = "standard,grpc"
String proxy_types = "envoy,nginx"

String aws_model_list = "cloud,AV2500,AV1000"
String aws_instance_list = "c5.xlarge,c5.2xlarge,c5.4xlarge,c5.9xlarge,c5a.xlarge,c5a.2xlarge,c5a.4xlarge,c5a.9xlarge"

String gcp_model_list = "GV2500,GV1000"
String gcp_instance_list = "n1-standard-4"

String azure_model_list = "MV2500,MV1000"
String azure_instance_list = "MI1,MI2,MI3"

String v1000_test_list = "http_0.1G,https_0.1G"
String v2500_test_list = "http_0.5G,https_0.5G"
String cloud_test_list = "http_2G,https_1.5G"

String instances = "c5.xlarge,c5.2xlarge,c5.4xlarge,c5.9xlarge,c5a.xlarge,c5a.2xlarge,c5a.4xlarge,c5a.9xlarge,n1-standard-4,MI1,MI2,MI3"
String models = "cloud,AV2500,AV1000,GV2500,GV1000,MV2500,MV1000"
String tests = "http_0.5G,https_0.5G,http_0.1G,https_0.1G,http_2G,https_1.5G"

// Methods to build groovy scripts to populate data
String buildScript(List values){
    return "return $values"
}

private Map getTerraformDeployments(boolean isCouldForm) {
    Map deployments = [:]
    return deployments
}

private Map getCloudFormationDeployments(boolean isCloudForm, String build) {
    Map deployments = [:]
    return deployments
}


// =============================================== Properties zone ===============================================
properties([
    parameters([
        [$class: 'StringParameterDefinition', name: 'BUILD_NAME', defaultValue: 'Cloud performance automation'],
        [$class: 'ExtendedChoiceParameterDefinition',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_SINGLE_SELECT',
         name: 'Build_version',
         value: build_versions,
         defaultValue: "14 or earlier"],
        [$class: 'ExtendedChoiceParameterDefinition',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_SINGLE_SELECT',
         name: 'Provider',
         value: providers,
         defaultValue: "aws"],
        [$class: 'ExtendedChoiceParameterDefinition',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_SINGLE_SELECT',
         name: 'Mode',
         value: gw_modes,
         defaultValue: "standard"],
        [$class: 'ExtendedChoiceParameterDefinition',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_SINGLE_SELECT',
         name: 'Proxy',
         value: proxy_types,
         defaultValue: "envoy"],
        [$class: 'ExtendedChoiceParameterDefinition',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_SINGLE_SELECT',
         name: 'Model',
         value: models,
         defaultValue: "AV2500"],
        [$class: 'ExtendedChoiceParameterDefinition',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_SINGLE_SELECT',
         name: 'Instance',
         value: instances,
         defaultValue: "c5.xlarge"],
        [$class: 'ExtendedChoiceParameterDefinition',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_MULTI_SELECT',
         name: 'TESTS_TO_RUN',
         value: tests.join(','),
         visibleItemCount: 3],
        [$class: 'ExtendedChoiceParameterDefinition',
         name: 'SETUP_SELECTION',
         description: 'Setups to run tests against',
         multiSelectDelimiter: ',',
         quoteValue: false,
         saveJSONParameterToFile: false,
         type: 'PT_MULTI_SELECT',
         value: 'mx_gw_byol,mxha_gw_byol',
         defaultValue: "mx_gw_byol",
         visibleItemCount: 3],
        [$class: 'StringParameterDefinition', name: 'MX_IP', defaultValue: "10.188.0.0", description: "Specify if you want to run tests on already deployed env"],
        [$class: 'StringParameterDefinition', name: 'GW_IP', defaultValue: "10.188.0.0", description: "Specify if you want to run tests on already deployed env"],
        [$class: 'StringParameterDefinition', name: 'LB_DNS_NAME', defaultValue: "", description: "Specify if you want to run tests on already deployed env"],
        [$class: 'StringParameterDefinition',
         name: 'BUILD',
         defaultValue: '14.7.0.10_latest',
         description: 'Build for auto deploy. Specify "latest" after underscore to automatically get the latest one.'],
        [$class: 'BooleanParameterDefinition', name: "DELETE_ENV", defaultValue: true],
        [$class: 'BooleanParameterDefinition', name: "DEPLOY_ENV", defaultValue: true],
        [$class: 'BooleanParameterDefinition',
         name: "CLOUD_FORMATION",
         defaultValue: true,
         description: "All AWS deployments will be done via CTT and CloudFormation."],
        [$class: 'BooleanParameterDefinition', name: "Support_HTTP2_on_GW", defaultValue: true],
        [$class: 'BooleanParameterDefinition', name: "ENABLE_TESTRAIL_INTEGRATION", defaultValue: false],
        [$class: 'StringParameterDefinition',
         name: 'TEST_PLAN_ID',
         defaultValue: "",
         description: "Specify to have results published into particular Test Plan in TestRail"],
    ])
])

pipeline {
    agent any
stages {
   stage('Echo selection'){
    steps {
        echo 'Build name\t' + params.BUILD_NAME
        echo 'Build version\t' + params.BUILD
        echo 'Provider\t' + params.Provider
        echo 'Model\t' + params.Model
        echo 'Instance\t' + params.Instance
        echo 'SETUP_SELECTION\t' + params.SETUP_SELECTION
        echo 'TESTS_TO_RUN\t' + params.TESTS_TO_RUN
        echo 'MX_IP\t' + params.MX_IP
        echo 'GW_IP\t' + params.GW_IP
        echo 'Provider\t' + params.Provider
        echo 'Provider\t' + params.Provider
        echo 'Delete after completion\t' + params.DELETE_ENV
        echo 'DEPLOY_ENV\t' + params.DEPLOY_ENV
        echo 'CLOUD_FORMATION\t' + params.CLOUD_FORMATION
        echo 'HTTP2_ENABLED\t' + params.HTTP2_ENABLED
        echo 'ENABLE_TESTRAIL_INTEGRATION\t' + params.ENABLE_TESTRAIL_INTEGRATION
        echo 'TEST_PLAN_ID\t' + params.TEST_PLAN_ID

        echo 'BUILD_NUMBER\t' + env.BUILD_NUMBER
        echo 'JOB_NAME\t' + env.JOB_NAME
        echo 'JENKINS_HOME\t' + env.JENKINS_HOME

      }
    }
   }
}
