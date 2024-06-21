# PowerShell: Microsoft Edge force install Progressive Web Apps (PWA)

<b>Objective:</b>

* Microsoft Edge force install Progressive Web Apps (PWA)
    * Install Twitter
    * Install Telegram
    * Install Spotify

<b>PowerShell snippet:</b>

```powershell
$apps =
ConvertTo-Json @(
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
    }
) -Compress

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
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

<b>PowerShell snippet with custom names for desktop items:</b>

```powershell
$apps =
ConvertTo-Json @(
    @{
        create_desktop_shortcut  = $true
        default_launch_container = "window"  
        url                      = "https://twitter.com/"
        custom_name              = "X Not Twitter"
    },
    @{
        create_desktop_shortcut  = $true
        default_launch_container = "window"  
        url                      = "https://web.telegram.org/"
        custom_name              = "Telegram"
    },
    @{
        create_desktop_shortcut  = $true
        default_launch_container = "window"  
        url                      = "https://open.spotify.com/"
        custom_name              = "Spotify"
    }
) -Compress

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
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