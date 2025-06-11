pipeline {
    agent { label 'windows-agent' }
    environment {
        WEB_ROOT = "C:\\inetpub\\wwwroot\\WebApplication1"
        PUBLISH_DIR = "${env.WORKSPACE}\\publish_output"
    }
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-cred', branch: 'main', url: 'https://github.com/kanhu1997/webapp1.git'
            }
        }
        stage('Restore') {
            steps {
                bat 'dotnet restore webapp1.sln'
            }
        }
        stage('Build') {
            steps {
                bat 'dotnet build webapp1.sln --configuration Release'
            }
        }
        stage('Publish') {
            steps {
                bat 'dotnet publish WebApplication1/WebApplication1.csproj --configuration Release --output publish_output'
            }
        }
        stage('Deploy') {
            steps {
                powershell '''
                $source = "${env:PUBLISH_DIR}"
                $destination = "${env:WEB_ROOT}"
                Copy-Item -Path "$source\\*" -Destination $destination -Recurse -Force
                Write-Host "Web app files deployed to ${env:WEB_ROOT}"
                '''
            }
        }
        stage('Verify Deployment') {
            steps {
                powershell '''
                $dll = Join-Path "${env:WEB_ROOT}" "WebApplication1.dll"
                if (Test-Path $dll) {
                    Write-Host "✅ Web app deployed successfully."
                } else {
                    Write-Error "❌ Deployment failed. WebApplication1.dll not found."
                }
                Write-Host "\n--- Listing files in web root ---"
                Get-ChildItem -Path "${env:WEB_ROOT}" -Recurse | Select-Object FullName
                '''
            }
        }
    }
    post {
        failure {
            echo '🚨 Deployment failed. Check logs for details.'
        }
        success {
            echo '🎉 Deployment completed successfully.'
        }
    }
}
