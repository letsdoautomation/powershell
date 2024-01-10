# PowerShell: Configure Microsoft Edge Start page quick links

<b>Objectives:</b>

* Configure Microsoft Edge Start page quick links

<b>PowerShell snippet:</b>

```powershell
$links =
@{
    url    = "https://google.com"
    title  = "Google"
    pinned = $true
},
@{
    url    = "https://youtube.com"
    title  = "Youtube"
    pinned = $true
} | ConvertTo-Json -Compress

$settings = 
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 1 # Remove default sites
    Name  = "NewTabPageHideDefaultTopSites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = $links
    Name  = "NewTabPageManagedQuickLinks"
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
