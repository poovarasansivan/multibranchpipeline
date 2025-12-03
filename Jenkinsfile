@Library('ps-jenkins-sharedlib') _

pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                sh 'echo "Build step running..."'
            }
        }

        stage('Main Branch Only Tasks') {
            when {
                branch 'main'
            }
            steps {
                script {
                    deployPipeline("prod")  
                }
            }
        }
    }
    
    post {
        success {
            echo "Pipeline Successful for ${env.BRANCH_NAME}"
        }
    }
}
