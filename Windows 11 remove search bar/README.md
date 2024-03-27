# PowerShell: Windows 11 remove search bar

<b>Objectives:</b>

* Remove search bar for current user
* Remove search bar for all existing current users and new ones

<b>For single user:</b>

```powershell
$settings = [PSCustomObject]@{
    Path  = "Software\Microsoft\Windows\CurrentVersion\Search"
    Value = 0
    Name  = "SearchboxTaskbarMode"
} | group Path

foreach ($setting in $settings) {
    $registry = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::CurrentUser.CreateSubKey($setting.Name, $true)
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

<b>For all existing and new users:</b>

``` powershell
ni "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\SearchboxTaskbarMode" | New-ItemProperty -Name "StubPath" -Value 'REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce" /v SearchboxTaskbarMode /t REG_SZ /d "cmd /c REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Search" /v SearchboxTaskbarMode /t REG_DWORD /d 0 /f'
```

## Related videos

<b>Windows Registry</b>

[Windows Registry: Run and RunOnce](https://youtu.be/zgFzCq5uEPw) <br />
[Windows Registry: Active Setup](https://youtu.be/HrVJ7wdvfmo)

<b>Other powershell videos</b>

[PowerShell playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)