# PowerShell: Google Chrome disable first run pop-up windows

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

## Related videos

<b>Other powershell videos</b>

[PowerShell playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)
[Google Chrome playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4TOfhZyNOqTyrpRS6HXFqjW)
