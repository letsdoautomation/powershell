# PowerShell: Firefox change default search engine

<b>Objective:</b>

* Firefox change default search engine

<b>Notes:</b> Works <b>ONLY</b> for Firefox ESR <br />

<img src="img/note.png" width=40% height=40%>

<b>Download Firefox ESR</b> <br/>

[Firefox ESR](https://www.mozilla.org/en-US/firefox/all/#product-desktop-esr)

<b>Providers:</b> </br>

* <b>google:</b> https://www.google.com/search?q={searchTerms}
* <b>duckduckgo:</b> https://duckduckgo.com/?q={searchTerms}
* <b>bing:</b> https://www.bing.com/search?q={searchTerms}
* <b>yahoo:</b> https://search.yahoo.com/search?p={searchTerms}
* <b>yandex:</b> https://yandex.com/search/?text={searchTerms}

<b>PowerShell set duckduckgo as default search provider:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines"
    Value = "duckduckgo"
    Name  = "Default"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "https://duckduckgo.com/?q={searchTerms}"
    Name  = "URLTemplate"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "https://duckduckgo.com/favicon.ico"
    Name  = "IconURL"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "duck"
    Name  = "Alias"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "GET"
    Name  = "Method"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "duckduckgo"
    Name  = "Name"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "Description"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "Encoding"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "PostData"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "SuggestURLTemplate"
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

<b>PowerShell set bing as default search provider:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines"
    Value = "bing"
    Name  = "Default"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "https://www.bing.com/search?q={searchTerms}"
    Name  = "URLTemplate"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "https://www.bing.com/favicon.ico"
    Name  = "IconURL"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "bing"
    Name  = "Alias"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "GET"
    Name  = "Method"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = "bing"
    Name  = "Name"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "Description"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "Encoding"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "PostData"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\SearchEngines\Add\1"
    Value = ""
    Name  = "SuggestURLTemplate"
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