# PowerShell: Google Chrome force install Progressive Web Apps (PWA)

<b>Objective:</b>

* Google Chrome force install Progressive Web Apps (PWA)
    * Install Twitter
    * Install Telegram
    * Install Spotify

<b>Notes:</b> Works for Windows Pro and Enterprise

<b>PowerShell snippet:</b>

```powershell
$apps =
@{
    create_desktop_shortcut  = $true
    default_launch_container = "window"  
    url                      = "https://twitter.com/"
},
@{
    create_desktop_shortcut  = $true
    default_launch_container = "window"  
    url                      = "https://web.telegram.org/"
},
@{
    create_desktop_shortcut  = $true
    default_launch_container = "window"  
    url                      = "https://open.spotify.com/"
} | ConvertTo-Json -Compress

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = $apps
    Name  = "WebAppInstallForceList"
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