# PowerShell: Microsoft Edge allow only certain pages

<b>Objective:</b>

* Microsoft Edge allow only certain pages
    * Block everything
    * Allow google.com
    * Allow youtube.com
    * Allow duckduckgo.com

<b>Notes:</b> Works for Windows Pro and Enterprise

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ # block everything
    Path  = "SOFTWARE\Policies\Microsoft\Edge\URLBlocklist"
    Value = "*"
    Name  = "1"
},
[PSCustomObject]@{ # allow google.com
    Path  = "SOFTWARE\Policies\Microsoft\Edge\URLAllowlist"
    Value = "google.com"
    Name  = ++$count
},
[PSCustomObject]@{ # allow google.com
    Path  = "SOFTWARE\Policies\Microsoft\Edge\URLAllowlist"
    Value = "duckduckgo.com"
    Name  = ++$count
},
[PSCustomObject]@{ # allow youtube.com
    Path  = "SOFTWARE\Policies\Microsoft\Edge\URLAllowlist"
    Value = "youtube.com"
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