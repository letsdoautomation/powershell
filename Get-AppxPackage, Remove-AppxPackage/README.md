# PowerShell: Get-AppxPackage, Remove-AppxPackage

<b>Documentation:</b>

* [Get-AppxPackage](https://learn.microsoft.com/en-us/powershell/module/appx/get-appxpackage?view=windowsserver2022-ps)
* [Remove-AppxPackage](https://learn.microsoft.com/en-us/powershell/module/appx/remove-appxpackage?view=windowsserver2022-ps)

<b>Objectives:</b>

* <b>Use powershell to:</b>
   * List installed Windows store apps
   * Remove Windows store apps for existing users only(not relly.. it kinda works for new users also)

<b>Get all app packages:</b>

```powershell
Get-AppxPackage -AllUsers | ?{!$_.NonRemovable} | select Name, PackageFullName
```

<b>Get-AppxPackage available properties:</b>

```powershell

   TypeName: Microsoft.Windows.Appx.PackageManager.Commands.AppxPackage

Name                   MemberType Definition
----                   ---------- ----------
Architecture           Property   Windows.System.ProcessorArchitecture Architecture {get;}
Dependencies           Property   Microsoft.Windows.Appx.PackageManager.Commands.AppxPackage[] Dependencies {get;}
InstallLocation        Property   string InstallLocation {get;}
IsBundle               Property   bool IsBundle {get;}
IsDevelopmentMode      Property   bool IsDevelopmentMode {get;}
IsFramework            Property   bool IsFramework {get;}
IsPartiallyStaged      Property   bool IsPartiallyStaged {get;}
IsResourcePackage      Property   bool IsResourcePackage {get;}
Name                   Property   string Name {get;}
NonRemovable           Property   bool NonRemovable {get;}
PackageFamilyName      Property   string PackageFamilyName {get;}
PackageFullName        Property   string PackageFullName {get;}
PackageUserInformation Property   System.Collections.Generic.IEnumerable..
Publisher              Property   string Publisher {get;}
PublisherId            Property   string PublisherId {get;}
ResourceId             Property   string ResourceId {get;}
SignatureKind          Property   Windows.ApplicationModel.PackageSignatureKind SignatureKind {get;}
Status                 Property   Microsoft.Windows.Appx.PackageManager.Commands.AppxStatus Status {get;}
Version                Property   string Version {get;}
```

<b>Remove single app package for all existing user:</b>

```powershell
Remove-AppxPackage "Microsoft.OutlookForWindows_1.2024.111.100_x64__8wekyb3d8bbwe" -AllUsers 
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

Get-AppxPackage -AllUsers | ?{$_.name -in $app_packages} | Remove-AppxPackage -AllUsers
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

Get-AppxPackage -AllUsers | ?{$_.name -notin $app_packages -and !$_.NonRemovable} | select name
```

# Related videos

[Export-StartLayout, Import-StartLayout]() <br />
[Get-AppxProvisionedPackage, Remove-AppxProvisionedPackage]()