🧑‍🎓 This stupid repo is created for educational purposes only [_and only for me!_]
Because if CloudProtection is enabled you won't go far! x_x
🤷‍♂️ But if you are in the same boat with me [_having a Windows VM that restarts WinDefend features every time on StartUp_], then you are at the right time, in the right place [probably] :)

# DisAVling
🚮 This is the repository that disables Windows Defender and its features on startup.

# AV disabler [disAVle.txt]
Execute this messy sh1t with PS:
```
echo "Set-MpPreference -DisableRealtimeMonitoring $true;"> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -SubmitSamplesConsent 2;" >> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableArchiveScanning $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableEmailScanning $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableBehaviorMonitoring $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableScriptScanning $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableIntrusionPreventionSystem $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableIOAVProtection $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableCloudProtection $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -SubmitSamplesConsent NeverSend;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -DisableBlockAtFirstSeen $true;">> C:\Users\Public\DisAVle.txt;
echo "Set-MpPreference -MAPSReporting Disabled;">> C:\Users\Public\DisAVle.txt;
echo "NetSh Advfirewall set allprofiles state off;">> C:\Users\Public\DisAVle.txt;
echo "Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False;" >> C:\Users\Public\DisAVle.txt
```
Or just download the `disAVle.txt` file from repository into `C:\Users\Public\`.

# Task Scheduler
Execute this sh1t with PS:
```
$Action = New-ScheduledTaskAction -Execute "powershell.exe" `
    -Argument '-Command "GET-Content -Raw C:\Users\Public\DisAVle.txt | IEX"' `
    -WorkingDirectory "C:\Windows\System32"

$TriggerStartup = New-ScheduledTaskTrigger -AtStartup
$TriggerLogon   = New-ScheduledTaskTrigger -AtLogOn

$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

$Settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -StartWhenAvailable

$Task = New-ScheduledTask -Action $Action -Trigger $TriggerStartup, $TriggerLogon -Principal $Principal -Settings $Settings

Register-ScheduledTask -TaskName "DisAVleOnBoot" -InputObject $Task
```

### _Win or lose, blame yourself, if anything happens to you, because you are still alive_ 🫶🏻
<!-- [Giorgi Dograshvili]([url](https://www.linkedin.com/in/giorgi-dograshvili)) -->
