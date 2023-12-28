# PowerShell: Install Google Chrome Extensions
### Documentation and download
<b>Documentation links:</b>

* [Alternative extension installation methods](https://developer.chrome.com/docs/extensions/how-to/distribute/install-extensions)

<b>Objectives:</b>

* Install chrome extensions

<b>Extension store:</b>

* [Chrome Webstore](https://chromewebstore.google.com/category/extensions)

```powershell
$extensions = ''
$key_path = "Software\Policies\Google\Chrome\ExtensionInstallForcelist"

$registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($key_path, $true)

$extensions | %{
    if($null -eq $registry){
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($key_path, $true)
        $registry.SetValue("1", $_)
    }
    else{
        $values = $registry.GetValueNames().ForEach({$registry.GetValue($_)})
        if($_ -notin $values){
            $maximum = $registry.GetValueNames().Where({$_ -match "\d"}) | measure -maximum | select -expand maximum
            $maximum += 1
            $registry.SetValue($maximum, $_)
        }
    }
}

$registry.Dispose()
```