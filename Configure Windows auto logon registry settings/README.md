# PowerShell: Configure Windows auto logon registry settings

<b>Documentation:</b>

* [Turn on automatic logon in Windows](https://learn.microsoft.com/en-us/troubleshoot/windows-server/user-profiles-and-logon/turn-on-automatic-logon)
* [Working with registry entries](https://learn.microsoft.com/en-us/powershell/scripting/samples/working-with-registry-entries?view=powershell-5.1)

<b>Objectives:</b>

* Configure auto logon for local user
* Configure auto logon for domain user
* Remove auto logon registry settings

<b>Notes:</b>

* Auto logon user requires password
* Auto logon stores password as plain text in registry 

<b>Configure auto logon using powershell for local user:</b>

```powershell
$auto_logon_settings = 
[PSCustomObject]@{ # Enable auto logon
    Name  = "AutoAdminLogon"
    Value = "1"
},
[PSCustomObject]@{ # Configure username
    Name  = "DefaultUserName"
    Value = "local_user"
},
[PSCustomObject]@{ # Configure password
    Name  = "DefaultPassword"
    Value = "password55"
}

foreach ($item in $auto_logon_settings) {
    $item_property = @{
        Path  = 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon'
        Name  = $item.name
        Value = $item.value
    }
    New-ItemProperty @item_property -Force | Out-Null
}
```

<b>Configure autologon using powershell for domain user:</b>

```powershell
$auto_logon_settings = 
[PSCustomObject]@{ # Enable auto logon
    Name  = "AutoAdminLogon"
    Value = "1"
},
[PSCustomObject]@{ # Configure username
    Name  = "DefaultUserName"
    Value = "domain_user"
},
[PSCustomObject]@{ # Configure password
    Name  = "DefaultPassword"
    Value = "password55#"
},
[PSCustomObject]@{ # Configure domain
    Name  = "DefaultDomainName"
    Value = "ad.letsdoautomation.com"
}

foreach ($item in $auto_logon_settings) {
    $item_property = @{
        Path  = 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon'
        Name  = $item.name
        Value = $item.value
    }
    New-ItemProperty @item_property -Force | Out-Null
}
```

<b>Remove auto logon configuration in registry:</b>

```powershell
$auto_logon_settings = 
[PSCustomObject]@{ # Disable auto logon
    Name  = "AutoAdminLogon"
    Value = "0"
},
[PSCustomObject]@{ # Configure username
    Name  = "DefaultUserName"
    Value = ""
},
[PSCustomObject]@{ # Configure domain
    Name  = "DefaultDomainName"
    Value = ""
}

foreach ($item in $auto_logon_settings) {
    $item_property = @{
        Path  = 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon'
        Name  = $item.name
        Value = $item.value
    }
    New-ItemProperty @item_property -Force | Out-Null
}

Remove-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon' -Name "DefaultPassword" -ea SilentlyContinue
```

## Related videos

<b>Other powershell videos</b>

[PowerShell playlist](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)