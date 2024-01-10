# PowerShell: Disable Microsoft Edge DNS over HTTPS

<b>Objective:</b>

* Disable Microsoft Edge DNS over HTTPS

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = "off"
    Name  = "DnsOverHttpsMode"
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
