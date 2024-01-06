# PowerShell: Microsoft Edge remove Import Favorites button

<b>Objective:</b>

* Microsoft Edge remove Import Favorites button

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0 # 1 - Enable, 0 - Disable
    Name  = "ImportFavorites"
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