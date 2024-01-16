# PowerShell: Firefox disable quick links in Home page

<b>Objective:</b>

* Firefox disable quick links in Home page

<b>PowerShell snippet:</b>

```powershell
$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 1
    Name  = "Locked"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 1
    Name  = "Search"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "Highlights"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "Pocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "Snippets"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "SponsoredPocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "SponsoredTopSites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "TopSites"
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
