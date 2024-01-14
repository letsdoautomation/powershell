# PowerShell: Firefox allow only certain web pages

<b>Documentation:</b>

* [Match Patterns](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Match_patterns)

<b>Objective:</b>

* Firefox allow only certain web pages
    * Block everything
    * Allow google.com
    * Allow youtube.com
    * Allow duckduckgo.com

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ # block everything
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\WebsiteFilter\Block"
    Value = "<all_urls>"
    Name  = "1"
},
[PSCustomObject]@{ # allow facebook
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\WebsiteFilter\Exceptions"
    Value = "*://*.google.com/*"
    Name  = ++$count
},
[PSCustomObject]@{ # allow instagram
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\WebsiteFilter\Exceptions"
    Value = "*://*.youtube.com/*"
    Name  = ++$count
},
[PSCustomObject]@{ # allow instagram
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\WebsiteFilter\Exceptions"
    Value = "*://*.duckduckgo.com/*"
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
