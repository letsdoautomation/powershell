# PowerShell: Windows 11 disable widgets in taskbar

```powershell
$settings = [PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Dsh"
    Value = 0
    Name  = "AllowNewsAndInterests"
} | group Path

foreach ($setting in $settings) {
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

## Related videos

<b>Other powershell videos</b>

[PowerShell playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)