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
                    echo "Download code form Git"    
                    git 'https://github.com/falgunbhalsodb/maven-project.git'
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
            post {
                success {
                    echo "Build Project"
                    copyArtifacts fingerprintArtifacts: true, projectName: 'Deploy-Dev', selector: lastSuccessful()
                    archiveArtifacts '**/*.war'
                    build 'Deploy-Prod'
                    

                }
            }
        }

        stage ('prod') {
            steps {
                echo "This is Prod Stage"
                timeout(time: 60, unit: 'SECONDS') {
                    input 'Do you want to process to Prod'
                    }
                    build 'Deploy-Prod'
            }
        } 
    }
}
