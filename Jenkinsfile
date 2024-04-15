pipeline {
    agent any

    parameters {
        string(name: 'apiName', defaultValue: '', description: 'Enter the API name')
        string(name: 'apiVersion', defaultValue: '', description: 'Enter the API version')
        choice(name: 'environment', choices: ['dev', 'prod'], description: 'Select the environment')
    }

    stages {
        stage('Checkout Base Repository') {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/base']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'base']], userRemoteConfigs: [[url: 'https://github.com/tharinduorg/train-apis.git']])
                }
            }
        }
        
        stage('Checkout Environment Repository') {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/base']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: "${params.environment}"]], userRemoteConfigs: [[url: 'https://github.com/tharinduorg/train-apis.git']])
                }
            }
        }

        stage('Validate API with rules') {
            steps {
                script{
                    sh "echo 'Validating API'"
                    sh "echo 'API Name: ${params.apiName}'"
                    sh "echo 'API Version: ${params.apiVersion}'"
                    sh "echo 'Environment: ${params.environment}'"
                }
            }
        }
        stage('Deploy API to ${params.environment}') {
            steps {
                script {
                    sh "echo 'Deploying API to ${params.environment}'"
                }
                script{
                    sh "/media/tharindud/dev/forrester/apictl/apictl import api -f base/${params.apiName}/${params.apiVersion}/${params.apiName}-${params.apiVersion} -e ${params.environment} --params ${params.environment}/${params.apiName}/${params.apiVersion}  -k --preserve-provider=false --update"
                }
            }
        }
    }
}
