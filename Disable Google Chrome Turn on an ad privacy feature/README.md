# PowerShell: Disable Google Chrome Turn on an ad privacy feature

<b>Objectives:</b>

* Disable Turn on an ad privacy feature message
* Disable Privacy Sandbox Ad measurement 
* Disable Privacy Sandbox Ad topics
* Disable Privacy Sandbox Site-suggested ads

<img src="img/privacysandbox.png" width=40% height=40%>

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0 # 0 disable, 1 enable
    Name  = "PrivacySandboxPromptEnabled" # notification
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0 # 0 disable, 1 enable
    Name  = "PrivacySandboxAdMeasurementEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0 # 0 disable, 1 enable
    Name  = "PrivacySandboxAdTopicsEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0 # 0 disable, 1 enable
    Name  = "PrivacySandboxSiteEnabledAdsEnabled"
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
