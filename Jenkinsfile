pipeline {
    agent {
        label 'windows-agent'
    }
    environment {
        IIS_ROOT = 'C:\\appserver'
    }
    stages {
        stage('Clone repository') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        stage('Restore Dependencies') {
            steps {
                script {
                    bat 'dotnet restore WebApplication1/WebApplication1.csproj'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    bat 'dotnet build WebApplication1/WebApplication1.csproj'
                }
            }
        }
        stage('Publish') {
            steps {
                script {
                    bat 'dotnet publish WebApplication1/WebApplication1.csproj -c Release -o ./publish'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    bat 'xcopy /E /Y /I .\\publish\\* "%IIS_ROOT%\\"'
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            // Remove the publish directory after deployment
            bat 'if exist .\\publish rmdir /s /q .\\publish'
        }
    }
}