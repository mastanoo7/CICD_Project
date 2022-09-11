pipeline{
    agent any
    stages{
        stage("SonarQualityCheck"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew  clean sonarqube'
                    }

                }
            
            }
        }
    }
}
