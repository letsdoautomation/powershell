# PowerShell: Microsoft Edge open web pages on startup

<b>Objectives:</b>

* Microsoft Edge open web pages on startup

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<img src="img/note.png" width=40% height=40%>

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 4 # 5 - Open New Tab Page, 
              # 1 - Restore the last session, 
              # 4 - Open a list of URLs
    Name  = "RestoreOnStartup"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge\RestoreOnStartupURLs"
    Value = "https://youtube.com"
    Name  = ++$count
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge\RestoreOnStartupURLs"
    Value = "https://google.com"
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