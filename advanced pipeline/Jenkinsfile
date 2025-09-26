pipeline {   
    agent any
    tools {
        maven 'maven-3.9'
    }
    parameters {
       string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod') 
       choice(name: 'VERSION', choices: ['1.0', '1.1', '1.2', '1.3'], description: 'Select the version to deploy')
       booleanParam(name: 'executeTests', true, description: '')
    }

    environment {
        NEW_VERSION = '1.3.0'
        SERVER_CREDENTIALS = credentials('server-credentials') // Use sensitive credentials securely
    }
    stages {
        stage("test") {
            when {
                expression {
                    params.executeTests == true
                }
            }
            steps {
                script {
                    echo "Testing the application...."
                    echo "building version is ${NEW_VERSION}"
                }
            }
        }
        
        stage("build") {
            when {
                expression {
                    BRANCH_NAME == 'main' || BRANCH_NAME == 'master' && CODE_CHANGES == true
                    
                }
            }
            steps {
                script {
                    echo "Building the application...."
                        // Print Java version in CI to confirm JDK used by agent
                        sh 'java -version'
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo "Deploying the application...."
                    echo "deploying with user ${SERVER_CREDENTIALS_USR}"
                    withCredentials([
                        usernamePassword(credentialsId: 'server-credentials', passwordVariable: 'PWD', usernameVariable: 'USER')
                    ]) {
                        sh "whoami"
                    }

                }
            }
        }               
    }
} 
