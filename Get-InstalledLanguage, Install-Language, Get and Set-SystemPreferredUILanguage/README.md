# PowerShell: Get-InstalledLanguage, Install-Language, Get and Set-SystemPreferredUILanguage

<b>Documentation:</b>

* [Install-Language](https://learn.microsoft.com/en-us/powershell/module/languagepackmanagement/install-language?view=windowsserver2022-ps)
* [Get-InstalledLanguage](https://learn.microsoft.com/en-us/powershell/module/languagepackmanagement/get-installedlanguage?view=windowsserver2022-ps)
* [Set-SystemPreferredUILanguage](https://learn.microsoft.com/en-us/powershell/module/languagepackmanagement/set-systempreferreduilanguage?view=windowsserver2022-ps)

<b>Use powershell to:</b>

* Check installed languages
* Install French language

<b>List installed languages:</b>

```powershell
Get-InstalledLanguage
```

<b>Show System Preferred UI language:</b>

```powershell
Get-SystemPreferredUILanguage
```

<b>Install language:</b>

```powershell
$install_language = @{
    Language       = "fr-FR"
    CopyToSettings = $true # This parameter changes French language as default language for all users
}
Install-Language @install_language
```

# Install language if needed
```powershell
$language = "fr-FR"

$current_preferred_ui_lanague = Get-SystemPreferredUILanguage

if($language -ne $current_preferred_ui_lanague){
    $language_installed = Get-InstalledLanguage $language
    if(!$language_installed){
            $install_language = @{
            Language       = $language
        }
        Install-Language @install_language
    }
    Set-SystemPreferredUILanguage $language
}
```

### Related videos

<b>Regional powershell commands:</b>

[Get-Culture, Set-Culture](https://youtu.be/gS4BckaTKto) <br />
[Get-TimeZone, Set-TimeZone](https://youtu.be/fmoIfJwvH-I) <br />
[Get-WinHomeLocation, Set-WinHomeLocation](https://youtu.be/yWp_1L8YDoQ) <br />
[Get-WinSystemLocale, Set-WinSystemLocale](https://youtu.be/rCGlh3hp1fI) <br />
[Get-WinUserLanguageList, Set-WinUserLanguageList](https://youtu.be/Bhl-rLB8g28) <br />

<b>Other powershell videos:</b>

[PowerShell](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)