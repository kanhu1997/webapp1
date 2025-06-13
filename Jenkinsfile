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
                    // Delete all files and folders in IIS_ROOT
                    bat 'if exist "%IIS_ROOT%\\*" del /F /Q "%IIS_ROOT%\\*"'
                    bat 'for /d %%i in ("%IIS_ROOT%\\*") do rmdir /s /q "%%i"'
                    // Copy new files
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
        success {
            mail to: 'kanhucharandash55@gmail.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [#${env.BUILD_NUMBER}]'",
                 body: """Good news job is sucessfully comlated!

The Jenkins job '${env.JOB_NAME}' (build #${env.BUILD_NUMBER}) has completed **successfully**.

Check console output at: ${env.BUILD_URL}

- Jenkins"""
        }
        failure {
            mail to: 'kanhucharandash55@gmail.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [#${env.BUILD_NUMBER}]'",
                 body: """ Job Failed!

The Jenkins job '${env.JOB_NAME}' (build #${env.BUILD_NUMBER}) has **FAILED**.

Check the logs and fix the issue.

Console output: ${env.BUILD_URL}

- Jenkins"""
        }
    }
}