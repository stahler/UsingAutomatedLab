# UsingAutomatedLab
Following is how I set up my Active Directory Lab using AutomatedLab.
1. Ensure you have Hyper-V set up on the host.  <br>_Install necessitates a reboot._
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/9647705f-dcb4-4f9a-a11e-3d8574330ab6)

2. Install the PowerShell module.<br>_Perform all of the steps in an elevated session._<br>
```PowerShell
Find-Module AutomatedLab | Install-Module -Scope AllUsers
```
3.  Create and populate a new labsources folder
```PowerShell
 New-LabSourcesFolder
```
This creates the necessary folder structure.
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/9b51c690-09cb-4f1a-a963-8ff4d1c67c02)

4.  Copy your ISO files to the ISO directory (C:\LabSources\ISOs)<br>
_I had issues with evaluation editions of the Operating Systems._

5.  Verify the ISOs (pay attention to the OperatingSystemName)
```PowerShell
Get-LabAvailableOperatingSystem
```
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/bb3b3c98-35a4-47f1-adbf-74dff2338cdd)

6. Create a lab definition
```PowerShell
New-LabDefinition -Name TestLab -DefaultVirtualizationEngine HyperV
```
This does quite a few things  including pulling in the SysInternals suite
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/fce4f036-82ec-4fd1-9de3-6b17c5ff377c)

7. Add machines to our Lab Definition
```PowerShell
$DomainController = @{
    Name            = 'DC01'
    Memory          = '1GB'
    OperatingSystem = 'Windows Server 2022 Standard (Desktop Experience)'
    Roles           = 'RootDC'
    DomainName      = 'FatBeard.com'
}
Add-LabMachineDefinition @DomainController

$server = @{
    Name            = 'Server01'
    Memory          = '1GB'
    OperatingSystem = 'Windows Server 2022 Standard (Desktop Experience)'
    DomainName      = 'FatBeard.com'
}
Add-LabMachineDefinition @Server
```

8.  Create the lab!
```PowerShell
Install-Lab
# Take a break, you deserve it! Plus this takes a few minutes.
```
9.  You can get a high-level summary of what was created
```PowerShell
Show-LabDeploymentSummary -Detailed
```
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/62249470-eb56-4830-8c20-a2e72651e760)

10.  Open Hyper-V to verify
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/3589f518-8d2a-4f43-b549-35708418b3ec)

11. Want to connect to a machine from the commandline?
```PowerShell
Connect-LabVM DC01
```

12.  Want to delete the lab?
```PowerShell
Remove-Lab -Name <Name of Lab>
```

 
