# PowerShell: Firefox disable first run page and default bookmarks

<b>Objective:</b>

* Firefox disable first run page and default bookmarks

<b>PowerShell snippet:</b>

```powershell
$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = 1
    Name  = "NoDefaultBookmarks"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = "" 
    Name  = "OverrideFirstRunPage"
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
