# PowerShell: Configure Brave Browser DNS over HTTPS

<b>Objectives:</b>

* Configure Brave Browser DNS over HTTPS

<b>PowerShell snippet:</b>

```powershell
# secure - The "secure" mode will only send DNS-over-HTTPS queries and will fail to resolve on error.
# automatic -The "automatic" mode will send DNS-over-HTTPS queries first if a DNS-over-HTTPS server is available and may fallback to sending insecure queries on error.
# off - The "off" mode will disable DNS-over-HTTPS.
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Value = "secure"
    Name  = "DnsOverHttpsMode" # secure, automatic or off
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
