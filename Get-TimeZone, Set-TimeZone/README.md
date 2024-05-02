# PowerShell: Get-TimeZone, Set-TimeZone

<b>Documentation: </b>

[Get-TimeZone](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-timezone?view=powershell-5.1) <br />
[Set-TimeZone](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-timezone?view=powershell-5.1)


<b>Get currently set time zone:</b>

```powershell
Get-TimeZone
```

<b>List availble time zones:</b>

```powershell
Get-TimeZone -ListAvailable
```

<b>Get time zone by name:</b>

```powershell
Get-TimeZone "*utc*"
```

<b>Get time zone by DisplayName:</b>

```powershell
Get-TimeZone -ListAvailable | ?{$_.DisplayName -like "*Paris*"}
```

<b>Set time zone using Name parameter:</b>

```powershell
Set-TimeZone "FLE Standard Time"
```

<b>Set time zone using DisplayName parameter:</b>

```powershell
Get-TimeZone -ListAvailable | ?{$_.DisplayName -like "*Paris*"} | Set-TimeZone
```

### Related videos

<b>Regional powershell commands:</b>

[Get-Culture, Set-Culture](https://youtu.be/gS4BckaTKto) <br />
[Get-InstalledLanguage, Install-Language, Get and Set-SystemPreferredUILanguage](https://youtu.be/eN-56mOM5GQ) <br />
[Get-WinHomeLocation, Set-WinHomeLocation](https://youtu.be/yWp_1L8YDoQ) <br />
[Get-WinSystemLocale, Set-WinSystemLocale](https://youtu.be/rCGlh3hp1fI) <br />
[Get-WinUserLanguageList, Set-WinUserLanguageList](https://youtu.be/Bhl-rLB8g28) <br />

<b>Other powershell videos:</b>

[PowerShell](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RDyVzbV0_kpXCScTMgUw_A)