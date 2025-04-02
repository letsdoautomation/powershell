# PowerShell: Pause Windows 11 updates

<b>Update settings location in registry:</b>

```batch
SOFTWARE\Microsoft\WindowsUpdate\UX\Settings
SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings
```

<b>Pause Windows Updates:</b>

```powershell
$pause_for_days = 30

$date = Get-Date

$pause_start = $date.ToUniversalTime().ToString( "yyyy-MM-ddTHH:mm:ssZ" )
$pause_end = $date.AddDays($pause_for_days).ToUniversalTime().ToString( "yyyy-MM-ddTHH:mm:ssZ" )

$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
    Name  = "PauseFeatureUpdatesEndTime"
    Value = $pause_end
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
    Name  = "PauseQualityUpdatesEndTime"
    Value = $pause_end
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
    Name  = "PauseUpdatesExpiryTime"
    Value = $pause_end
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
    Name  = "PauseFeatureUpdatesStartTime"
    Value = $pause_start
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
    Name  = "PauseQualityUpdatesStartTime"
    Value = $pause_start
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
    Name  = "PauseUpdatesStartTime"
    Value = $pause_start
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings"
    Name  = "PausedFeatureDate"
    Value = $pause_start
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings"
    Name  = "PausedQualityDate"
    Value = $pause_start
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings"
    Name  = "PausedFeatureStatus"
    Value = 1
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings"
    Name  = "PausedQualityStatus"
    Value = 1
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

<b>Resume Windows Updates:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Name = 'PauseFeatureUpdatesEndTime'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UX\Settings'
},
[PSCustomObject]@{
    Name = 'PauseQualityUpdatesEndTime'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UX\Settings'
},
[PSCustomObject]@{
    Name = 'PauseUpdatesExpiryTime'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UX\Settings'
},
[PSCustomObject]@{
    Name = 'PauseFeatureUpdatesStartTime'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UX\Settings'
},
[PSCustomObject]@{
    Name = 'PauseQualityUpdatesStartTime'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UX\Settings'
},
[PSCustomObject]@{
    Name = 'PauseUpdatesStartTime'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UX\Settings'
},
[PSCustomObject]@{
    Name = 'PausedFeatureDate'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings'
},
[PSCustomObject]@{
    Name = 'PausedQualityDate'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings'
},
[PSCustomObject]@{
    Name = 'PausedFeatureStatus'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings'
},
[PSCustomObject]@{
    Name = 'PausedQualityStatus'
    Path = 'SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings'
}

foreach ($setting in ($settings | group Path)) {
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        continue
    }

    foreach ($item in $setting.Group.Where({ $null -ne $registry.GetValue($_.name) })) {
        $registry.DeleteValue($item.name, $true)
    }
    $registry.Dispose()
}
```