# PowerShell: Export-StartLayout, Import-StartLayout alternatives for Windows 11

<b>Objectives:</b>

* <b>Method 1:</b> Modify start menu for <b>NEW</b> users by replicating Intune registry settings with powershell.
    * Can be used for new computers with Windows configuration designer or other computer provisioning tools
* <b>Method 2:</b> Modify start menu for all users on the computer by deploying start2.bin file to all users on the computer

## Method 1

<b>Configure start menu with MDM policy via registry(for new users only):</b>

```powershell
$apps = 
"Microsoft.Windows.Explorer",
"Microsoft.Windows.ControlPanel",
"Microsoft.WindowsCalculator_8wekyb3d8bbwe!app",
"Microsoft.Paint_8wekyb3d8bbwe!app",
"C:\Windows\System32\cmd.exe",
"C:\Windows\regedit.exe"

$start_pins = @{
    pinnedList = foreach ($app in $apps) {
        if ($app -match "\w:\\") {
            @{
                desktopAppLink = $app
            }
        }
        elseif ($app -match "Microsoft\.Windows\.") {
            @{
                desktopAppId = $app
            }
        }
        else {
            @{
                packagedAppId = $app
            }
        }
    }
} | ConvertTo-Json -Compress

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\PolicyManager\current\device\Start"
    Value = $start_pins
    #Value = '{ "pinnedList": [] }' # only for remove everything
    Name  = "ConfigureStartPins"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\PolicyManager\current\device\Start"
    Value = 1
    Name  = "ConfigureStartPins_ProviderSet"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\PolicyManager\current\device\Start"
    Value = "B5292708-1619-419B-9923-E5D9F3925E71"
    Name  = "ConfigureStartPins_WinningProvider"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\PolicyManager\providers\B5292708-1619-419B-9923-E5D9F3925E71\default\Device\Start"
    Value = $start_pins
    #Value = '{ "pinnedList": [] }' # only for remove everything
    Name  = "ConfigureStartPins"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\PolicyManager\providers\B5292708-1619-419B-9923-E5D9F3925E71\default\Device\Start"
    Value = 1
    Name  = "ConfigureStartPins_LastWrite"
} | group Path

foreach ($setting in $settings) {
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | % {
        $registry.SetValue($_.name, $_.value)
    }
    $registry.Dispose()
}
```

<b>Show packagedAppId for Windows store apps:</b>

```powershell
Get-AppxPackage | select @{n='name';e={"$($_.PackageFamilyName)!app"}} | ?{$_.name -like "**"}
```

## Method 2

<b>start2.bin file location:</b>

* %LOCALAPPDATA%\Packages\Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy\LocalState\

<b>Deploy start2.bin file from current user to all other users on the computer:</b>

```powershell
[System.IO.FileInfo]$start_layout = "$($env:LOCALAPPDATA)\Packages\Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy\LocalState\start2.bin"

ls "C:\Users\" -Attributes Directory -Force | ?{$_.FullName -notin $env:USERPROFILE, $env:PUBLIC -and $_.Name -notin "All Users", "Default User"} | %{

    [System.IO.DirectoryInfo]$destination = "$($_.FullName)\AppData\Local\Packages\Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy\LocalState"

    if(!$destination.Exists){
        $destination.Create()
    }

    $start_layout.CopyTo("$($destination)\start2.bin", $true)
}
```

# Related videos

[PowerShell: Get-AppxPackage, Remove-AppxPackage](https://youtu.be/SrjEV6rkoEQ) <br />
[PowerShell: Using Get-AppxProvisionedPackage, Remove-AppxProvisionedPackage to modify Online image](https://youtu.be/SevFgIkzAKk)
