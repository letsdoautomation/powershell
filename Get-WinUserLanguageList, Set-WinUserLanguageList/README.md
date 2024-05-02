# PowerShell: Get-WinUserLanguageList, Set-WinUserLanguageList

<b>Documentation:</b>

[Get-WinUserLanguageList](https://learn.microsoft.com/en-us/powershell/module/international/get-winuserlanguagelist?view=windowsserver2022-ps) <br />
[Set-WinUserLanguageList](https://learn.microsoft.com/en-us/powershell/module/international/set-winuserlanguagelist?view=windowsserver2022-ps) <br />
[Copy-UserInternationalSettingsToSystem](https://learn.microsoft.com/en-us/powershell/module/international/copy-userinternationalsettingstosystem?view=windowsserver2022-ps) <br />
[Default input profiles (input locales) in Windows](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs?view=windows-11)

<b>Get-WinUserLanguageList:</b>

```powershell
Get-WinUserLanguageList
```

<b>Use Set-WinUserLanguageList to set fr-fr keyboard:</b>

```powershell
Set-WinUserLanguageList fr-fr -force -wa silentlycontinue
```

<b>Use Set-WinUserLanguageList to set multiple keyboards:</b>

```powershell
Set-WinUserLanguageList en-us, fr-fr -force -wa silentlycontinue
```

<b>Add keyboard to current keyboard list:</b>

```powershell
$keyboards = Get-WinUserLanguageList
$keyboards.add('hu-HU')
Set-WinUserLanguageList $keyboards -force -wa silentlycontinue
```

<b>Copy settings to new users and welcome screen:</b>

```powershell
Copy-UserInternationalSettingsToSystem -WelcomeScreen $True -NewUser $True
```

### Related videos

<b>Regional powershell commands:</b>

[Get-Culture, Set-Culture](https://youtu.be/gS4BckaTKto) <br />
[Get-InstalledLanguage, Install-Language, Get and Set-SystemPreferredUILanguage](https://youtu.be/eN-56mOM5GQ) <br />
[Get-TimeZone, Set-TimeZone](https://youtu.be/fmoIfJwvH-I) <br />
[Get-WinHomeLocation, Set-WinHomeLocation](https://youtu.be/yWp_1L8YDoQ) <br />
[Get-WinSystemLocale, Set-WinSystemLocale](https://youtu.be/rCGlh3hp1fI) <br />

<b>Other powershell videos:</b>

[PowerShell](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)