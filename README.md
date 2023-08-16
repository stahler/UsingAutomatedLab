# UsingAutomatedLab
Following is how I set up my Active Directory Lab using [AutomatedLab](https://automatedlab.org/en/latest/).
1. Ensure you have Hyper-V set up on the host.  <br>_Install necessitates a reboot._
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/12bd3346-9b3f-40ec-92bd-b2e5841647b5)

2. Install the PowerShell module.<br>_Perform all of the steps in an elevated session._<br>
```PowerShell
Find-Module AutomatedLab | Install-Module
```
3.  Create and populate a new labsources folder
```PowerShell
 New-LabSourcesFolder
```
This creates the necessary folder structure.
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/1174cc1a-f19f-489c-a206-5f0b8b5f4d64)

4.  Copy your ISO files to the ISO directory (C:\LabSources\ISOs)<br>
_I had issues with evaluation editions of the Operating Systems._

5.  Verify the ISOs (pay attention to the OperatingSystemName)
```PowerShell
Get-LabAvailableOperatingSystem
```
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/4203e3da-d817-4fdf-a871-4da09824e0b7)

6. Create a lab definition
```PowerShell
New-LabDefinition -Name TestLab -DefaultVirtualizationEngine HyperV
```
This does quite a few things  including pulling in the SysInternals suite

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
9.  You can get a high-level summary of what was created<br>
_The default credentials are Administrator/Somepass1._
```PowerShell
Show-LabDeploymentSummary -Detailed
```
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/d12fdc03-7a5f-4595-9c3a-8342a73a2a75)

10.  Open Hyper-V to verify
![image](https://github.com/stahler/UsingAutomatedLab/assets/1991193/411a4b01-6522-41f9-b76b-af624e889db7)

11. Want to connect to a machine from the commandline?
```PowerShell
Connect-LabVM DC01
```

12.  Want to delete the lab?
```PowerShell
Remove-Lab -Name <Name of Lab>
```

 
