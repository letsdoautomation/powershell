# PowerShell: Brave Browser disable Brave Wallet

<b>Objectives:</b>

* Brave Browser disable Brave Wallet

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Value = 1 # 0 - Enable, 1 - Disable
    Name  = "BraveWalletDisabled"
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
