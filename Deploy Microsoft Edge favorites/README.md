# PowerShell: Deploy Microsoft Edge favorites

<b>Documentation:</b>

* [ManagedFavorites](https://learn.microsoft.com/en-us/DeployEdge/microsoft-edge-policies#managedfavorites)
* [Provision favorites and folders](https://learn.microsoft.com/en-us/deployedge/edge-learnmore-provision-favorites#provision-favorites-and-folders)

<b>Objective:</b>

* Deploy Microsoft Edge favorites

<b>PowerShell snippet:</b>

```powershell
$managed_favorites = @{
    toplevel_name = "Managed favorites"
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
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = $managed_favorites
    Name  = "ManagedFavorites"
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
