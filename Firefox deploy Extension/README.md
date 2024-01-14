# PowerShell: Firefox deploy Extension

<b>Objective:</b>

* Firefox deploy Extension
    * [Firefox add-ons](https://addons.mozilla.org/en-US/firefox/extensions/)

<b>PowerShell snippet:</b>

```powershell
$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Extensions\Install"
    Value = "https://addons.mozilla.org/firefox/downloads/file/4216633/ublock_origin-1.55.0.xpi"
    Name  = ++$count
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Extensions\Install"
    Value = "https://addons.mozilla.org/firefox/downloads/file/4211087/bitwarden_password_manager-2023.12.1.xpi"
    Name  = ++$count
} | group Path

foreach($setting in $settings){
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | %{
        $registry.SetValue($_.name, $_.value)
    }
    $registry.Dispose()
}
```
