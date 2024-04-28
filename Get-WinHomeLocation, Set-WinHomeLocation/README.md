# PowerShell: Get-WinHomeLocation, Set-WinHomeLocation

<b>Documentation:</b>

[Get-WinHomeLocation](https://learn.microsoft.com/en-us/powershell/module/international/get-winhomelocation?view=windowsserver2022-ps) <br />
[Set-WinHomeLocation](https://learn.microsoft.com/en-us/powershell/module/international/set-winhomelocation?view=windowsserver2022-ps) <br />
[Copy-UserInternationalSettingsToSystem](https://learn.microsoft.com/en-us/powershell/module/international/copy-userinternationalsettingstosystem?view=windowsserver2022-ps) <br />
[Table of Geographical Locations](https://learn.microsoft.com/en-us/windows/win32/intl/table-of-geographical-locations?redirectedfrom=MSDN)

<b>Get current home location:</b>

```powershell
Get-WinHomeLocation
```

<b>Set home location to:</b>

```powershell
Set-WinHomeLocation 84
```

<b>Copy settings to new users and welcome screen:</b>

```powershell
Copy-UserInternationalSettingsToSystem -WelcomeScreen $True -NewUser $True
```

### Related videos

<b>Regional powershell commands:</b>

[Get-Culture, Set-Culture]() <br />
[Get-InstalledLanguage, Install-Language, Get and Set-SystemPreferredUILanguage]() <br />
[Get-TimeZone, Set-TimeZone]() <br />
[Get-WinHomeLocation, Set-WinHomeLocation]() <br />
[Get-WinSystemLocale, Set-WinSystemLocale]() <br />
[Get-WinUserLanguageList, Set-WinUserLanguageList]() <br />

<b>Other powershell videos:</b>

[PowerShell](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)