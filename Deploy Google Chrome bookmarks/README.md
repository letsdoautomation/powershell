# PowerShell: Deploy Google Chrome bookmarks

<b>Objective:</b>

* Deploy Google Chrome bookmarks

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<b>PowerShell snippet:</b>

```powershell
$managed_bookmarks = @{
    toplevel_name = "Managed bookmarks"
},
@{
    name = "YouTube"
    url = "https://youtube.com"
},
@{
    children = 
    @{
        name = "DuckDuckGo"
        url = "https://duckduckgo.com"
    },
    @{
        name = "Bing"
        url = "https://bing.com"
    },
    @{
        name = "Google"
        url = "https://google.com"
    }
    name = "Search Engines"
},
@{
    children = 
    @{
        name = "Instagram"
        url = "https://instagram.com"
    },
    @{
        name = "Facebook"
        url = "https://facebook.com"
    }
    name = "Social media"
} | ConvertTo-JSON -depth 4 -Compress

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = $managed_bookmarks
    Name  = "ManagedBookmarks"
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