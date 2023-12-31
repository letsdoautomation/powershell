# PowerShell: Change default Google Chrome search provider

<b>Objectives:</b>

* Change default Google Chrome search provider

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<img src="img/note.png" width=40% height=40%>

<b>Providers:</b> </br>

* <b>google:</b> https://www.google.com/search?q={searchTerms}
* <b>duckduckgo:</b> https://duckduckgo.com/?q={searchTerms}
* <b>bing:</b> https://www.bing.com/search?q={searchTerms}
* <b>yahoo:</b> https://search.yahoo.com/search?p={searchTerms}
* <b>yandex:</b> https://yandex.com/search/?text={searchTerms}

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ # Enable default search providers
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 1
    Name  = "DefaultSearchProviderEnabled"
},
[PSCustomObject]@{ # Set search provider url
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = "https://duckduckgo.com/?q={searchTerms}"
    Name  = "DefaultSearchProviderSearchURL"
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