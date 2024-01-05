# PowerShell: Install Microsoft Edge add-ons

<b>Objectives:</b>

* Install Microsoft Edge add-ons

<b>Extension store:</b>

* [Microsoft Edge Extensions](https://microsoftedge.microsoft.com/addons/Get-started-with-microsoft-edge-extensions)

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallForcelist"
    Value = "elhekieabhbkpmcefcoobjddigjcaadp"
    Name  = ++$count
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallForcelist"
    Value = "cnlefmmeadmemmdciolhbnfeacpdfbkd"
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