# PowerShell: Block Brave Browser access to a list of URLs

<b>Objectives:</b>

* Block Brave Browser access to a list of URLs
    * facebook
    * instagram

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ # block facebook
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave\URLBlocklist"
    Value = "facebook.com"
    Name  = ++$count
},
[PSCustomObject]@{ # block instagram
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave\URLBlocklist"
    Value = "instagram.com"
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