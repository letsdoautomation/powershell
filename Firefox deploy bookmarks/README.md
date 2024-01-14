# PowerShell: Firefox deploy bookmarks

<b>Objective:</b>

* Firefox deploy bookmarks
    * youtube.com
    * google.com
    * social media
        * twitter
        * facebook

<b>PowerShell snippet:</b>

```powershell
$settings = 
[PSCustomObject]@{ # Youtube
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\1"
    Value = ""
    Name  = "Title" # Mandatory
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\1"
    Value = "https://www.youtube.com"
    Name  = "URL" # Mandatory
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\1"
    Value = "toolbar" # toolbar | menu
    Name  = "Placement" 
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\1"
    Value = "https://www.youtube.com/favicon.ico"
    Name  = "Favicon"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\1"
    Value = ""
    Name  = "Folder"
},
[PSCustomObject]@{ # Google
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\2"
    Value = ""
    Name  = "Title"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\2"
    Value = "https://www.google.com"
    Name  = "URL"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\2"
    Value = "toolbar"
    Name  = "Placement" 
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\2"
    Value = "https://www.google.com/favicon.ico"
    Name  = "Favicon"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\2"
    Value = ""
    Name  = "Folder"
},
[PSCustomObject]@{ # Twitter
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\3"
    Value = "Twitter"
    Name  = "Title"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\3"
    Value = "https://www.twitter.com"
    Name  = "URL"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\3"
    Value = "toolbar"
    Name  = "Placement" 
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\3"
    Value = "https://www.twitter.com/favicon.ico"
    Name  = "Favicon"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\3"
    Value = "Social Media"
    Name  = "Folder"
},
[PSCustomObject]@{ # Facebook
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\4"
    Value = "Facebook"
    Name  = "Title"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\4"
    Value = "https://www.facebook.com"
    Name  = "URL"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\4"
    Value = "toolbar"
    Name  = "Placement" 
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\4"
    Value = "https://www.facebook.com/favicon.ico"
    Name  = "Favicon"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Bookmarks\4"
    Value = "Social Media"
    Name  = "Folder"
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
