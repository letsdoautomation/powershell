# PowerShell: Disable Google Chrome website notifications

[bennish web-notifications](https://www.bennish.net/web-notifications.html)

<b>Objective:</b>

* Disable Google Chrome website notifications

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<b>PowerShell snippet:</b>

```powershell

# 1 - Allow sites to show desktop notifications
# 2 - Do not allow any site to show desktop notifications
# 3 - Ask every time a site wants to show desktop notifications

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 2
    Name  = "DefaultNotificationsSetting"
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