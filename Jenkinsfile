        stage('Deploy') {
            steps {
                // Overwrite web.config with a valid version before deployment
                bat 'copy /Y web.config publish_output\\web.config'
                powershell '''
                $source = "${env:PUBLISH_DIR}"
                $destination = "${env:WEB_ROOT}"
                # Ensure logs directory exists for stdout logging
                $logs = Join-Path $destination "logs"
                if (!(Test-Path $logs)) { New-Item -ItemType Directory -Path $logs | Out-Null }
                Copy-Item -Path "$source\\*" -Destination $destination -Recurse -Force
                Write-Host "Web app files deployed to ${env:WEB_ROOT}"
                '''
                // The following step is removed:
                // powershell '''
                // Import-Module WebAdministration
                // Set-WebConfigurationProperty -pspath 'MACHINE/WEBROOT/APPHOST' -filter "system.webServer/aspNetCore/environmentVariables" -name "." -value @{name="ASPNETCORE_ENVIRONMENT";value="Development"} -location "Default Web Site"
                // '''
            }
        }