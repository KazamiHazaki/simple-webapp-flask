pipeline {
    agent any
    environment {
        SONARQUBE_ID = credentials('sonarserver_token')

    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/KazamiHazaki/simple-webapp-flask.git'
            }
        }
        
        stage('SonarQube Analysis 2') {

          steps {
            withSonarQubeEnv('Sonarqube-DeployAja') {
                sh"${tool name: 'Sonarscanner-DeployAja', type: 'hudson.plugins.sonar.SonarRunnerInstallation'}/bin/sonar-scanner -X -Dsonar.token=\${SONARQUBE_ID} -Dsonar.projectKey=flask-test"
                }
            sleep(10)
            }

          }

        stage('Quality Gate') {
            steps {
                // Add steps to perform your quality gate check
                // For example, you might use waitForQualityGate
                script {
                def qg = waitForQualityGate()
                
                if (qg.status != 'OK') {
                    currentBuild.result = 'FAILURE'
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
                }
            }
        }          
        
    
        
    }

}
