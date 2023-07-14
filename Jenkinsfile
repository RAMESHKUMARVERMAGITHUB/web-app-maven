pipeline {
    
    agent any
    tools {
      maven 'maven'
    }
    
    
    stages {
        stage ('checkout') {
            steps{
                git branch: 'main', url: 'https://github.com/zielotechtest/web-app-maven.git'
            }
        }
        
        stage ('Build') {
            steps{
                sh 'mvn clean package checkstyle:checkstyle'
            }
        }
        stage ('Test') {
            steps{
                sh 'mvn test'
            }
        }
        stage ('Code quality analysis') {
            steps{
                recordIssues(tools: [checkStyle()])
            }
        }
        
        stage ('Code Coverage') {
            steps{
                jacoco maximumBranchCoverage: '80', maximumClassCoverage: '80', maximumComplexityCoverage: '80', maximumInstructionCoverage: '80', maximumLineCoverage: '80', maximumMethodCoverage: '80', minimumBranchCoverage: '80', minimumClassCoverage: '80', minimumComplexityCoverage: '80', minimumInstructionCoverage: '80', minimumLineCoverage: '80', minimumMethodCoverage: '80'
            }
        }
        
        stage ('Docker Build and Publish') {
            steps{
                // sh 'docker build -t web .'
                
                withCredentials([usernamePassword(credentialsId: 'DOCKER-CRED', passwordVariable: 'pswd', usernameVariable: 'uname')]) {
                    sh 'docker login -u $uname -p $pswd'
                    sh 'docker build -t web .'
                    sh 'docker tag web:latest nayakomprasad/demo123:v3'
                    sh 'docker push nayakomprasad/demo123:v3'
                }
                
            }
        }
        stage ('k8s') {
            steps{
                withAWS(credentials:'aws') {
                    sh 'aws s3 ls'
                    withKubeConfig([credentialsId: 'user1', serverUrl: 'https://api.k8s.my-company.com']) {
                      sh 'kubectl get nodes'
                    }
                }
            }
        }
        
        
        
    }
}
