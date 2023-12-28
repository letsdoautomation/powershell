# PowerShell: Enable This PC, Recycle Bin, User's Files, Control Panel and Network Desktop Icons

<b>Objectives:</b>

* Enable icons for single user
* Enable icons for all users

<b>Enable desktop icons for current user:</b>

```powershell
# Icons
$desktop_icons =
[pscustomobject]@{
    Entry = "{20D04FE0-3AEA-1069-A2D8-08002B30309D}"
    Description = "This PC"
},
[pscustomobject]@{
    Entry = "{5399E694-6CE5-4D6C-8FCE-1D8870FDCBA0}"
    Description = "Control Panel"
},
[pscustomobject]@{
    Entry = "{59031a47-3f72-44a7-89c5-5595fe6b30ee}"
    Description = "User's Files"
},
[pscustomobject]@{
    Entry = "{645FF040-5081-101B-9F08-00AA002F954E}"
    Description = "Recycle Bin"
},
[pscustomobject]@{
    Entry = "{F02C1A0D-BE21-4350-88B0-7367FC96EF3C}"
    Description = "Network"
}

# Enable icons for current user
$key_path = "Software\Microsoft\Windows\CurrentVersion\Explorer\HideDesktopIcons\NewStartPanel"
$hkcu = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey($key_path, $true)

$desktop_icons | %{
    "Enabling '$($_.Description)' desktop icon" | Out-Host
    $hkcu.SetValue($_.Entry, 0)
}

$hkcu.Dispose()
```

<b>Enable desktop icons for all users:</b>

```powershell
# Icons
$desktop_icons =
[pscustomobject]@{
    Entry = "{20D04FE0-3AEA-1069-A2D8-08002B30309D}"
    Description = "This PC"
},
[pscustomobject]@{
    Entry = "{5399E694-6CE5-4D6C-8FCE-1D8870FDCBA0}"
    Description = "Control Panel"
},
[pscustomobject]@{
    Entry = "{59031a47-3f72-44a7-89c5-5595fe6b30ee}"
    Description = "User's Files"
},
[pscustomobject]@{
    Entry = "{645FF040-5081-101B-9F08-00AA002F954E}"
    Description = "Recycle Bin"
},
[pscustomobject]@{
    Entry = "{F02C1A0D-BE21-4350-88B0-7367FC96EF3C}"
    Description = "Network"
}

# Enable icons for current user
$key_path = "Software\Microsoft\Windows\CurrentVersion\Explorer\HideDesktopIcons\NewStartPanel"
$hkcu = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey($key_path, $true)

$desktop_icons | %{
    "Enabling '$($_.Description)' desktop icon" | Out-Host
    $hkcu.SetValue($_.Entry, 0)
}

$hkcu.Dispose()

# Export current users settings
[System.IO.FileInfo]$destination = "$($env:ProgramData)\provisioning\Desktop_Icons.reg"

if(!$destination.Directory.Exists){
    $destination.Directory.Create()
}

$export = @{
    FilePath     = "reg"
    ArgumentList = "export" ,"HKCU\$($key_path)", $destination.FullName, "/y"
    Wait         = $true
    NoNewWindow  = $true
}
Start-Process @export

# Configure active setup to import registry file for all users
ni "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\importDesktopIcons" | New-ItemProperty -Name "StubPath" -Value ('reg import "{0}"' -f $destination.FullName)
```

## Related videos

<b>Windows Registry</b>

[Windows Registry: Run and RunOnce](https://youtu.be/zgFzCq5uEPw) <br />
[Windows Registry: Active Setup](https://youtu.be/HrVJ7wdvfmo)