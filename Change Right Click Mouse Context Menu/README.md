# PowerShell: Change Right Click Mouse Context Menu

<b>Objectives:</b>

* Change right click for single user
* Change right click for all users

<b>Enable old right click for single user:</b>

```powershell
ni "HKCU:\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" -Value "" -Force
```

<b>Enable old right click for all users:</b>

```powershell
ni "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\EnableOldRightClick" | New-ItemProperty -Name "StubPath" -Value 'REG ADD "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve'
```

## Related videos

<b>Windows Registry</b>

[Windows Registry: Run and RunOnce](https://youtu.be/zgFzCq5uEPw) <br />
[Windows Registry: Active Setup](https://youtu.be/HrVJ7wdvfmo)