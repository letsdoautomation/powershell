# PowerShell: Block Google Chrome access to a list of URLs

<b>Documentation</b>

* [URL blocklist filter format](https://support.google.com/chrome/a/answer/9942583?visit_id=638395414883072846-3752810819&p=url_blocklist_filter_format&rd=1#zippy=%2Curl-blocklist-examples)

<b>Objectives:</b>

* Block Google Chrome access to a list of URLs
    * facebook
    * instagram

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ # block facebook
    Path  = "SOFTWARE\Policies\Google\Chrome\URLBlocklist"
    Value = "facebook.com"
    Name  = ++$count
},
[PSCustomObject]@{ # block instagram
    Path  = "SOFTWARE\Policies\Google\Chrome\URLBlocklist"
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