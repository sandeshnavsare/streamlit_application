@Library('Shared') _
pipeline {
    agent any
    
    // environment{
    //     SONAR_HOME = tool "Sonar"
    // }
    
    parameters {
        string(name: 'Application_DOCKER_TAG', defaultValue: 'latest', description: 'Setting docker image for latest push')
    }
    
    stages {
        stage("Validate Parameters") {
            steps {
                script {
                    if  (params.Application_DOCKER_TAG == '') {
                        error("Application_DOCKER_TAG must be provided.")
                    }
                }
            }
        }
        stage("Workspace cleanup"){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        
        stage('Git: Code Checkout') {
            steps {
                script{
                    code_checkout("https://github.com/sandeshnavsare/streamlit_application.git","master")
                }
            }
        }
        
        

        stage("Docker: Build Images"){
            steps{
                script{
                    docker_build("streamlit-application","${params.Application_DOCKER_TAG}","sandeshnavsare")
                }
            }
        }
        
        stage("Docker: Push to DockerHub"){
            steps{
                script{
                    docker_push("streamlit-application","${params.Application_DOCKER_TAG}","sandeshnavsare")
                }
            }
        }
    }
    post{
        success{
                build job: "Streamlit-app-CD", parameters: [
                string(name: 'Application_DOCKER_TAG', value: "${params.Application_DOCKER_TAG}")
            ]
        }
    }
}

