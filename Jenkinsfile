pipeline{
    agent any
    stages{
        stage('Build Backend'){
            steps{
            bat 'mvn clean package -DskipTests=true'
            }
        }

        stage('Unit Tests'){
            steps{
            bat 'mvn test'
            }
        }

        stage('Sonar Analysis'){
            steps{
            bat "echo sonar"
            }
        }

        stage('QualityGate'){
            steps{
            bat "echo qualitygate"
            }
        }

        stage('Deploy Backend'){
            steps{
            deploy adapters: [tomcat8(credentialsId: 'login_tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

        stage('API Test'){
            steps{
                dir('api-test') {
                    git branch: 'main', credentialsId: 'github_hoqs', url: 'https://github.com/hoqs/teste-api-tasks'
                    bat 'mvn test'
                }
            }
        }
    }
}

