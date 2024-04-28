# PowerShell: Get-Culture, Set-Culture

<b>Documentation:</b>

[Get-Culture](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-culture?view=powershell-5.1) <br />
[Set-Culture](https://learn.microsoft.com/en-us/powershell/module/international/set-culture?view=windowsserver2022-ps) <br />
[Copy-UserInternationalSettingsToSystem](https://learn.microsoft.com/en-us/powershell/module/international/copy-userinternationalsettingstosystem?view=windowsserver2022-ps) <br />
[Default input profiles (input locales) in Windows](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs?view=windows-11)

<b>Get-Culture:</b>

```powershell
Get-Culture
```

<b>Use Set-Culture to set regional settings to French:</b>

```powershell
Set-Culture Fr-fr
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