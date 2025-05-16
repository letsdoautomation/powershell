
```powershell
[IO.DirectoryInfo]$provisioning = "$($env:PROGRAMDATA)\provisioning"

if(!$provisioning.Exists){
    $provisioning.Create()
}

@"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Control Panel\Desktop]
"WallPaper"="C:\\wallpaper.png"
  
"@ | Out-File "$($provisioning.FullName)\user-settings.reg" -Encoding unicode -Force

$configure_active_setup = @{
    Path        = "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\UserSettings"
    ErrorAction = "SilentlyContinue"
}

$configure_run_once = @{
    Name  = "StubPath"
    Value = 'REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce" /v ImportUserRegistry /d "REG IMPORT {0}" /f' -f "$($provisioning.FullName)\user-settings.reg"
}

ni @configure_active_setup | New-ItemProperty @configure_run_once
```

```powershell
$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\Windows\CurrentVersion\PersonalizationCSP"
    Name  = "DesktopImageStatus"
    Value = 1
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\Windows\CurrentVersion\PersonalizationCSP"
    Name  = "DesktopImagePath"
    Value = "C:\wallpaper.png"
}

foreach ($setting in ($settings | group Path)) {
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | % {
        $registry.SetValue($_.name, $_.value)
    }
    $registry.Dispose()
}
```