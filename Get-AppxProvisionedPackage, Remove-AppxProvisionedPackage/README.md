# PowerShell: Get-AppxProvisionedPackage, Remove-AppxProvisionedPackage

<b>Documentation:</b>

* [Get-AppxProvisionedPackage](https://learn.microsoft.com/en-us/powershell/module/dism/get-appxprovisionedpackage?view=windowsserver2022-ps)
* [Remove-AppxProvisionedPackage](https://learn.microsoft.com/en-us/powershell/module/dism/remove-appxprovisionedpackage?view=windowsserver2022-ps)

<b>Objectives:</b>

* <b>Use powershell to:</b>
   * List installed Windows store apps
   * Remove Windows store apps for new users only(not relly.. it kinda works for existing users also)

<b>List provisioning packages:</b>

```powershell
Get-AppxProvisionedPackage -online
```

<b>Get-AppxProvisionedPackage available properties:</b>

```powershell

   TypeName: Microsoft.Dism.Commands.AppxPackageObject

Name             MemberType Definition
----             ---------- ----------
Architecture     Property   uint32 Architecture {get;set;}
Build            Property   uint32 Build {get;set;}
DisplayName      Property   string DisplayName {get;set;}
InstallLocation  Property   string InstallLocation {get;set;}
LogLevel         Property   Microsoft.Dism.Commands.LogLevel LogLevel {get;set;}
LogPath          Property   string LogPath {get;set;}
MajorVersion     Property   uint32 MajorVersion {get;set;}
MinorVersion     Property   uint32 MinorVersion {get;set;}
Online           Property   bool Online {get;set;}
PackageName      Property   string PackageName {get;set;}
Path             Property   string Path {get;set;}
PublisherId      Property   string PublisherId {get;set;}
Regions          Property   string Regions {get;set;}
ResourceId       Property   string ResourceId {get;set;}
RestartNeeded    Property   bool RestartNeeded {get;set;}
Revision         Property   uint32 Revision {get;set;}
ScratchDirectory Property   string ScratchDirectory {get;set;}
SysDrivePath     Property   string SysDrivePath {get;set;}
WinPath          Property   string WinPath {get;set;}
```

<b>Remove single appx provisioned package:</b>

```powershell
Remove-AppxProvisionedPackage -PackageName "Microsoft.WindowsCalculator_2020.2103.8.0_neutral_~_8wekyb3d8bbwe" -Online -AllUser
```

<b>Remove multiple app packages for all existing user:</b>

```powershell
# Don't recommended removing
#"Microsoft.WindowsNotepad"
#"Microsoft.Paint"
#"Microsoft.WindowsCalculator"
#"Microsoft.XboxGamingOverlay"
#"Microsoft.Windows.Photos"

# System components
#"Microsoft.YourPhone"
#"Microsoft.Windows.DevHome"
#"Microsoft.GetHelp"
#"Microsoft.Getstarted"
#"Microsoft.WindowsStore"

$app_packages = 
"Microsoft.WindowsCamera",
"Clipchamp.Clipchamp",
"Microsoft.WindowsAlarms",
"Microsoft.549981C3F5F10", # Cortana
"Microsoft.WindowsFeedbackHub",
"microsoft.windowscommunicationsapps",
"Microsoft.WindowsMaps",
"Microsoft.ZuneMusic",
"Microsoft.BingNews",
"Microsoft.Todos",
"Microsoft.ZuneVideo",
"Microsoft.MicrosoftOfficeHub",
"Microsoft.OutlookForWindows",
"Microsoft.People",
"Microsoft.PowerAutomateDesktop",
"MicrosoftCorporationII.QuickAssist",
"Microsoft.ScreenSketch",
"Microsoft.MicrosoftSolitaireCollection",
"Microsoft.WindowsSoundRecorder",
"Microsoft.MicrosoftStickyNotes",
"Microsoft.BingWeather",
"Microsoft.Xbox.TCUI",
"Microsoft.GamingApp"

Get-AppxProvisionedPackage -online | ?{$_.DisplayName -in $app_packages} | Remove-AppxProvisionedPackage -Online -AllUser
```

<b>List appx packages that are not in the list</b>

```powershell
$app_packages = 
"Microsoft.WindowsCamera",
"Clipchamp.Clipchamp",
"Microsoft.WindowsAlarms",
"Microsoft.549981C3F5F10", # Cortana
"Microsoft.WindowsFeedbackHub",
"microsoft.windowscommunicationsapps",
"Microsoft.WindowsMaps",
"Microsoft.ZuneMusic",
"Microsoft.BingNews",
"Microsoft.Todos",
"Microsoft.ZuneVideo",
"Microsoft.MicrosoftOfficeHub",
"Microsoft.OutlookForWindows",
"Microsoft.People",
"Microsoft.PowerAutomateDesktop",
"MicrosoftCorporationII.QuickAssist",
"Microsoft.ScreenSketch",
"Microsoft.MicrosoftSolitaireCollection",
"Microsoft.WindowsSoundRecorder",
"Microsoft.MicrosoftStickyNotes",
"Microsoft.BingWeather",
"Microsoft.Xbox.TCUI",
"Microsoft.GamingApp",
"Microsoft.YourPhone", # System component
"Microsoft.Windows.DevHome", # System component
"Microsoft.GetHelp", # System component
"Microsoft.Getstarted", # System component
"Microsoft.WindowsStore", # System component
"Microsoft.WindowsNotepad", # Don't recommended removing
"Microsoft.Paint", # Don't recommended removing
"Microsoft.WindowsCalculator", # Don't recommended removing
"Microsoft.XboxGamingOverlay", # Don't recommended removing
"Microsoft.Windows.Photos" # Don't recommended removing

Get-AppxProvisionedPackage -online | ?{$_.DisplayName -notin $app_packages} | select DisplayName
```



# Related videos

[Get-AppxPackage, Remove-AppxPackage]() <br />
[Export-StartLayout, Import-StartLayout]()