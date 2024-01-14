# PowerShell: Firefox block access to a list of URLs

<b>Documentation:</b>

* [Match Patterns](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Match_patterns)

<b>Objective:</b>

* Firefox block access to a list of URLs
    * facebook
    * instagram

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ # block facebook
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\WebsiteFilter\Block"
    Value = "*://*.facebook.com/*"
    Name  = ++$count
},
[PSCustomObject]@{ # block instagram
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\WebsiteFilter\Block"
    Value = "*://*.instagram.com/*"
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
