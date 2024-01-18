# PowerShell: Firefox set Start, Home page and enable Home button

<b>Objective:</b>

* Firefox set Start, Home page and enable Home button

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = 1 # 1 | 0
    Name  = "ShowHomeButton" # Enable home button
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Homepage"
    Value = "https://google.com" # homepage
    Name  = "URL"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Homepage"
    Value = "homepage" # homepage | none | previous-session | homepage-locked
    Name  = "StartPage"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Homepage"
    Value = 0 # 1 | 0
    Name  = "Locked" # Allow to change homepage
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
