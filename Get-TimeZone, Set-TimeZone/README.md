# PowerShell: Get-TimeZone, Set-TimeZone

<b>Documentation: </b>

[Get-TimeZone](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-timezone?view=powershell-5.1) <br />
[Set-TimeZone](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-timezone?view=powershell-5.1)


<b>Get currently set timezone:</b>

```powershell
Get-TimeZone
```

<b>List availble timezones:</b>

```powershell
Get-TimeZone -ListAvailable
```

<b>Get timezone by name:</b>

```powershell
Get-TimeZone "*utc*"
```

<b>Get timezone by DisplayName:</b>

```powershell
Get-TimeZone -ListAvailable | ?{$_.DisplayName -like "*Paris*"}
```

```powershell
Set-TimeZone "FLE Standard Time"
```

```powershell
Get-TimeZone -ListAvailable | ?{$_.DisplayName -like "*Paris*"} | Set-TimeZone
```