        $Region = (Invoke-RestMethod "http://169.254.169.254/latest/dynamic/instance-identity/document").region
        $Bucketname = "aws-codedeploy-$Region"
        
        Function InstallCodeDeploy($Bucketname)
        {
        Import-Module AWSPowerShell
        New-Item -Path "c:\\Temp" -ItemType "directory" -Force
        powershell.exe -Command Read-S3Object -BucketName $Bucketname -Key latest/codedeploy-agent.msi -File c:\\Temp\\codedeploy-agent.msi
        c:\\Temp\\codedeploy-agent.msi /quiet /l c:\\Temp\\host-agent-install-log.txt
        }
        
        # Execution starts here 
        $Attempt = 1
        Do {
        $Timestamp = Get-Date
        Write-Host   "$Timestamp Attempting CodeDeploy install. Attempt # $Attempt"
        InstallCodeDeploy "$Bucketname"
        Start-Sleep -S 20
        $Service = Get-Service -Name codedeployagent -ErrorAction SilentlyContinue
        
        if($Service -ne $null)
        {
        Break
        }
        
        $Attempt++
        } While ( $Attempt -le 5 )
        
        $CodeDeploy = Get-Service -Name codedeployagent -ErrorAction SilentlyContinue | Select Name
        If ($CodeDeploy -ne $null)
        {
        Write-Host "codedeployagent is installed successfully"
        }
        else
        {
        Write-Host "Could not install codedeployagent after several attempts"
        }
