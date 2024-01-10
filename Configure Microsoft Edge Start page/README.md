# PowerShell: Configure Microsoft Edge Start page

<b>Objectives:</b>

* Configure Microsoft Edge Start page

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 3 # Remove background
    Name  = "NewTabPageAllowedBackgroundTypes"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0 # Disable APP launcher
    Name  = "NewTabPageAppLauncherEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0 # Remove news and other stuff
    Name  = "NewTabPageContentEnabled"
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
