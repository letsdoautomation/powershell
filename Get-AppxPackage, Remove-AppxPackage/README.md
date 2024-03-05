# PowerShell: Get-AppxPackage, Remove-AppxPackage

<b>Documentation:</b>

* [Get-AppxPackage](https://learn.microsoft.com/en-us/powershell/module/appx/get-appxpackage?view=windowsserver2022-ps)
* [Remove-AppxPackage](https://learn.microsoft.com/en-us/powershell/module/appx/remove-appxpackage?view=windowsserver2022-ps)

<b>Notes:</b>

   * This works only for existing users on the computer

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
$app_packages = 
"Microsoft.WindowsNotepad", # don't recommended removing
"Microsoft.Paint", # don't recommended removing
"Microsoft.WindowsCalculator", # don't recommended removing
"Microsoft.Windows.Photos",
"Microsoft.WindowsFeedbackHub",
"Microsoft.WindowsAlarms",
"Microsoft.WindowsMaps",
"Clipchamp.Clipchamp",
"Microsoft.OutlookForWindows",
"Microsoft.People",
"Microsoft.BingNews",
"Microsoft.WindowsCamera",
"Microsoft.BingWeather",
"Microsoft.Todos",
"Microsoft.MicrosoftOfficeHub",
"Microsoft.MicrosoftStickyNotes",
"Microsoft.PowerAutomateDesktop",
"Microsoft.MicrosoftSolitaireCollection",
"Microsoft.WindowsSoundRecorder",
"MicrosoftCorporationII.QuickAssist",
"Microsoft.YourPhone",
"Microsoft.Windows.DevHome",
"Microsoft.ZuneMusic",
"Microsoft.ZuneVideo",
"Microsoft.549981C3F5F10",
"microsoft.windowscommunicationsapps",
"Microsoft.ScreenSketch",
"Microsoft.Xbox.TCUI",
"Microsoft.GamingApp",
"Microsoft.GetHelp",
"Microsoft.Getstarted",
"SpotifyAB.SpotifyMusic_zpdnekdrzrea0!Spotify"

Get-AppxPackage -AllUsers | ?{$_.name -in $app_packages} | Remove-AppxPackage -AllUsers 
```