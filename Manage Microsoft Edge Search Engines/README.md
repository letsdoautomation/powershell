# PowerShell: Manage Microsoft Edge Search Engines

<b>Objective:</b>

* Manage Microsoft Edge Search Engines

<b>PowerShell snippet:</b>

```powershell
$search_engines =
@{
    is_default = $true
    keyword = "duck"
    name = "duckduckgo.com"
    search_url = "https://duckduckgo.com/?q={searchTerms}"
},
@{
    keyword = "bing"
    name = "bing.com"
    search_url = "https://www.bing.com/search?q={searchTerms}"
},
@{
    keyword = "google"
    name = "google.com"
    search_url = "https://www.google.com/search?q={searchTerms}"
} | ConvertTo-Json -Compress

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = $search_engines
    Name  = "ManagedSearchEngines"
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
