# PowerShell: Windows 11 Configure regional settings

<b>Documentation:</b>

[Default input profiles (input locales) in Windows](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs?view=windows-11) <br />
[Table of Geographical Locations](https://learn.microsoft.com/en-us/windows/win32/intl/table-of-geographical-locations?redirectedfrom=MSDN)

<b>PowerShell snippet:</b>

```powershell
# configure time zone

Get-TimeZone -ListAvailable | ?{$_.DisplayName -like "*Budapest*"} | Set-TimeZone

# configure regional/locale/keyboard settings
# Hungarian
$region = "hu-HU"

Set-Culture $region
Set-WinSystemLocale $region
Set-WinUserLanguageList $region, "en-us" -force -wa silentlycontinue
Set-WinHomeLocation 109

# copy regional settings to new user accounts and welcome screen

Copy-UserInternationalSettingsToSystem -WelcomeScreen $True -NewUser $True
```

### Related videos

<b>Regional powershell commands:</b>

[Get-Culture, Set-Culture](https://youtu.be/gS4BckaTKto) <br />
[Get-InstalledLanguage, Install-Language, Get and Set-SystemPreferredUILanguage](https://youtu.be/eN-56mOM5GQ) <br />
[Get-TimeZone, Set-TimeZone](https://youtu.be/fmoIfJwvH-I) <br />
[Get-WinHomeLocation, Set-WinHomeLocation](https://youtu.be/yWp_1L8YDoQ) <br />
[Get-WinSystemLocale, Set-WinSystemLocale](https://youtu.be/rCGlh3hp1fI) <br />
[Get-WinUserLanguageList, Set-WinUserLanguageList](https://youtu.be/Bhl-rLB8g28) <br />

<b>Other powershell videos:</b>

[PowerShell](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)