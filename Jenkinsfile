pipeline {
    agent any 
    tools {
                jdk "java8"
                maven "maven3.3.9"
    }
    stages {
                 
        
        stage ('build') {
            steps {
                echo "This is Build stage"
                sh label: '', script: 'mvn clean package checkstyle:checkstyle' 
            }
            post {
                success {
                    echo "Archive Artifacts"
                    archive '**/*.war'
                    echo "Publish junit report"
                    junit '**/target/surefire-reports/*.xml'
                    echo "Publish analasis result"
                    checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                }
            }
        }
    

        stage ('deploy-dev') {
            steps {
                echo "This is Dev-deploy stage"
            }
        }

        stage ('prod') {
            steps {
                echo "This is Prod Stage"
            }
        } 
    }
}