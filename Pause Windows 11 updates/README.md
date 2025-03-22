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
foreach ($item in 'PauseFeatureUpdatesEndTime', 'PauseQualityUpdatesEndTime', 'PauseUpdatesExpiryTime') {
    [PSCustomObject]@{
        Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
        Name  = $item
        Value = $pause_end
    }
}

$settings += 
foreach ($item in 'PauseFeatureUpdatesStartTime', 'PauseQualityUpdatesStartTime', 'PauseUpdatesStartTime') {
    [PSCustomObject]@{
        Path  = "SOFTWARE\Microsoft\WindowsUpdate\UX\Settings"
        Name  = $item
        Value = $pause_start
    }
}

$settings +=
foreach ($item in 'PausedFeatureDate', 'PausedQualityDate') {
    [PSCustomObject]@{
        Path  = "SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings"
        Name  = $item
        Value = $pause_start
    }
}

$settings +=
foreach ($item in 'PausedFeatureStatus', 'PausedQualityStatus') {
    [PSCustomObject]@{
        Path  = "SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings"
        Name  = $item
        Value = 1
    }
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