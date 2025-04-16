# PowerShell: Prevent extensions from being disabled or removed

</b>PowerShell snippet for locking extension:</b>

```powershell
$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Extensions\Locked"
    Value = "{54e2eb33-18eb-46ad-a4e4-1329c29f6e17}"
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

</b>PowerShell snippet for finding guid:</b>

```powershell
$extension = "Block Site"
$result = irm "https://addons.mozilla.org/api/v5/addons/search/?q=$($extension)"

foreach($item in $result.results){
    [PSCustomObject]@{
        name = $item.name.'en-us'
        guid = $item.guid
        url  = $item.url
    }
}
```