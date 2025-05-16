# PowerShell: Windows 11 configure wallpaper

<b>Objectives:</b>

* Configure wallpaper for single user
* Configure wallpaper for all users
  * Method that allows wallpaper to changed
  * Method that doesn't allow wallpaper to changed

<b>Configure wallpaper for single user:</b>

```powershell
$settings =
[PSCustomObject]@{
    Path  = "Control Panel\Desktop"
    Name  = "WallPaper"
    Value = "C:\wallpaper.png"
}

foreach ($setting in ($settings | group Path)) {
    $registry = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::CurrentUser.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | % {
        $registry.SetValue($_.name, $_.value)
    }
    $registry.Dispose()
}
```

<b>Configure wallpaper for multiple users(cannot be changed in the ui):</b>

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

<b>Configure wallpaper for multiple users(can be changed in the ui):</b>

```powershell
# Configuration for single user
$settings =
[PSCustomObject]@{
    Path  = "Control Panel\Desktop"
    Name  = "WallPaper"
    Value = "C:\wallpaper.png"
}

foreach ($setting in ($settings | group Path)) {
    $registry = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::CurrentUser.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | % {
        $registry.SetValue($_.name, $_.value)
    }
    $registry.Dispose()
}

# Configuration for other users
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

## Related videos

<b>Other powershell videos:</b>

* [PowerShell playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)
* [Windows Registry](https://www.youtube.com/playlist?list=PLVncjTDMNQ4TZrwwuYuZBZhpjs6YWw7sQ)