pipeline {
    agent any
parameters {
        string(name: 'BUILD_NAME', defaultValue: "Cloud performance automation")
        extendedChoice(name: 'CLOUD_PROVIDERS',
                description: 'Leave unselected to run for all providers',
                multiSelectDelimiter: ',',
                type: 'PT_MULTI_SELECT',
                value: 'AWS,GCP,Azure',
                defaultValue: "AWS",
                visibleItemCount: 3)
        extendedChoice(name: 'SETUP_SELECTION',
                description: 'Setups to run tests against',
                multiSelectDelimiter: ',',
                quoteValue: false,
                saveJSONParameterToFile: false,
                type: 'PT_MULTI_SELECT',
                value: 'mx_gw_byol,mxha_gw_byol',
                defaultValue: "mx_gw_byol",
                visibleItemCount: 3)
        extendedChoice(name: 'MODEL',
                description: 'SeS model type',
                multiSelectDelimiter: ',',
                quoteValue: false,
                saveJSONParameterToFile: false,
                type: 'PT_MULTI_SELECT',
                value: '2500',
                defaultValue: "cloud, 1000, 2500",
                visibleItemCount: 3)
        extendedChoice(name: 'INSTANCE_TYPE',
                description: 'Machine type',
                multiSelectDelimiter: ',',
                quoteValue: false,
                saveJSONParameterToFile: false,
                type: 'PT_MULTI_SELECT',
                value: 'C5.xlarge',
                defaultValue: "C5.xlarge, C5.2xlarge, C5.9xlarge",
                visibleItemCount: 3)
//         choice(name: 'SG_MODE', choices: ["active", "simulation"], description: 'Server Group operation mode')
//         choice(name: 'GRPC_SG_MODE', choices: ["active", "simulation"], description: 'gRPC Server Group operation mode')
//         choice(name: 'ENVOY_DEPLOYMENT', choices: ['standalone_envoy'], description: 'Envoy deployment model for WaaP tests')
//         string(name: 'ENVOY_IP', defaultValue: "10.188.0.0", description: "Specify if you want to run tests on already deployed env")
//         string(name: 'GRPC_GW_IP', defaultValue: "10.188.0.0", description: "Specify if you want to run tests on already deployed env")
        string(name: 'MX_IP', defaultValue: "10.188.0.0", description: "Specify if you want to run tests on already deployed env")
        string(name: 'GW_IP', defaultValue: "10.188.0.0", description: "Specify if you want to run tests on already deployed env")
        string(name: 'LB_DNS_NAME', defaultValue: "", description: "Specify if you want to run tests on already deployed env")
        string(name: 'BUILD', defaultValue: '14.7.0.10_latest',
                description: 'Build for auto deploy. Specify "latest" after underscore to automatically get the latest one.')
        extendedChoice(name: 'TESTS_TO_RUN',
                description: 'Select test suites to run. Leave unselected to run all the tests',
                multiSelectDelimiter: ' or ',
                quoteValue: false,
                saveJSONParameterToFile: false,
                type: 'PT_MULTI_SELECT',
                value: 'http_perf, http2_perf,waap_perf,mxha_perf',
                visibleItemCount: 10)
//         choice(name: 'LOG_LEVEL', choices: ['INFO', 'DEBUG'], description: '')
//         booleanParam(name: 'NIGHTLY_RUN', defaultValue: false)
        booleanParam(name: "DELETE_ENV", defaultValue: true)
        booleanParam(name: "DEPLOY_ENV", defaultValue: true)
        booleanParam(name: "CLOUD_FORMATION", defaultValue: true, description: "All AWS deployments will be done via CTT and CloudFormation.")
        booleanParam(name: 'HTTP2_ENABLED', defaultValue: true)
        booleanParam(name: 'ENABLE_TESTRAIL_INTEGRATION', defaultValue: false)
        string(name: 'TEST_PLAN_ID', defaultValue: '', description: 'Specify to have results published into particular Test Plan in TestRail')
    }
    options {
        buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '10'))
        timeout(time: 12, unit: 'HOURS')
        timestamps()
    }
    triggers {
        cron '7 11 * * *'
    }
    stages {
        stage('Greet') {
            steps {
                sh('echo Hello!')
            }
        }
    }
}
