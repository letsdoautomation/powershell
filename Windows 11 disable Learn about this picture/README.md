# PowerShell: Windows 11 disable Learn about this picture

<b>Remove "Learn about this picture" for single user:</b>

```powershell
ri 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace\{2cc5ca98-6485-489a-920e-b3e88a6ccce3}'
```

<b>Remove "Learn about this picture" for all users:</b>

```powershell
$registry_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\Active Setup\Installed Components\RemoveLearnAboutPicture"
    Value = "REG ADD `"HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`" /v RemoveLearnAboutPicture /d `"REG DELETE HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace\{2cc5ca98-6485-489a-920e-b3e88a6ccce3} /f`" /f"
    Name  = "StubPath"
}

# Apply registry settings
foreach ($setting in ($registry_settings | group Path)) {
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | % {
        if (!$_.Type) {
            $registry.SetValue($_.name, $_.value)
        }
        else {
            $registry.SetValue($_.name, $_.value, $_.type)
        }
    }
    $registry.Dispose()
}
```

# Related videos

<b>PowerShell:</b>

* [PowerShell playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)

<b>Reigstry:</b>

* [Windows Registry: Active Setup](https://youtu.be/HrVJ7wdvfmo)
* [Windows Registry: Run and RunOnce](https://youtu.be/zgFzCq5uEPw)