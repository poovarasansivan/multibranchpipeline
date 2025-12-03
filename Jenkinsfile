@Library('ps-jenkins-sharedlib') _
pipeline {
    agent any

    stages {

        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                echo " Workspace cleaned"
            }
        }

        stage('Code Checkout') {
            steps {
                echo " Checking out application source"
                checkout scm
            }
        }

        stage('Validate HTML') {
            steps {
                echo " Checking index.html availability"
                sh """
                    if [ ! -f app/index.html ]; then
                        echo " index.html NOT FOUND in app/ folder!"
                        exit 1
                    fi
                    echo " index.html found"
                """
            }
        }

        stage('Feature Build Steps') {
            when {
                expression { env.BRANCH_NAME.startsWith("feature/") }
            }
            steps {
                echo "Running build for feature branch: ${env.BRANCH_NAME}"
                sh 'echo "Static HTML validation successful!"'
            }
        }

        stage('Production Deployment Flow') {
            when {
                branch 'main'
            }
            steps {
                echo "🚀 Running production pipeline for main branch..."
                script {
                    deployPipeline("prod") 
                }

                sh """
                echo " Copying build files to deployment folder"
                mkdir -p deployment
                cp -r app/index.html deployment/
                echo "Deployment folder content:"
                ls -l deployment
                """
            }
        }
    }

    post {
        success {
            echo " Pipeline completed successfully for ${env.BRANCH_NAME}"
        }
        failure {
            echo " Pipeline failed for ${env.BRANCH_NAME}"
        }
    }
}
