pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("SonarQualityCheck"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew  clean sonarqube'
                    }
                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    } 
                }
            
            }
        }
        stage("docker build & docker push"){
            steps{
                script{
                    sh '''
                    docker build -t 34.174.212.1:8083/springapp:${VERSION} .
                    docker login -u admin -p mastan.123 34.174.212.1:8083
                    docker push 34.174.212.1:8083/springapp:${VERSION}
                    docker rmi 34.174.212.1:8083/springapp:${VERSION}
                    '''
                }
            }
        }
    }
}
