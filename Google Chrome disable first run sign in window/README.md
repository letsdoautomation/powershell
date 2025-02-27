# PowerShell: Google Chrome disable first run sign in window

<b>"Sign in to Chrome" screen:</b>

<img src="img/signin.png" width=40% height=40%>

<b>Disable sign in screen:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PromotionsEnabled"
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

<b>"AD Privacy" screen:</b>

<img src="img/privacysandbox.png" width=40% height=40%>

<b>Disable ad privacy features and startup sign in screen:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PromotionsEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PrivacySandboxPromptEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PrivacySandboxAdMeasurementEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PrivacySandboxAdTopicsEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
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
<b>Remove "ad privacy features and startup sign in screen" registry settings:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PromotionsEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxPromptEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxAdMeasurementEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxAdTopicsEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxSiteEnabledAdsEnabled"
} | group Path

foreach($setting in $settings){
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        continue
    }

    foreach($item in $setting.Group.Where({$null -ne $registry.GetValue($_.name)})){
        $registry.DeleteValue($item.name, $true)
    }
    $registry.Dispose()
}
```

## Related videos

<b>Other powershell videos</b>

[PowerShell playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)