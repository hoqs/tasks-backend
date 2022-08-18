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

        stage('Deploy Frontend'){
            steps{
            dir('frontend') {
                    git branch: 'master', credentialsId: 'github_hoqs', url: 'https://github.com/hoqs/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'login_tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                    
                }
            }
        }

        stage('Functional Test'){
            steps{
                dir('functional-test') {
                    credentialsId: 'github_hoqs', url: 'https://github.com/hoqs/tasks-functional-tests'
                    bat 'mvn test'
                }
            }
        }

        stage('Deploy Prod'){
            steps{
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }
    }
}



