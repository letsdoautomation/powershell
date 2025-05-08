# PowerShell: Creating viewer requested script


```powershell
[IO.DirectoryInfo]$provisioning = "$($env:PROGRAMDATA)\provisioning"

if(!$provisioning.Exists){
    $provisioning.Create()
}

@"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced]
"TaskbarAl"=dword:00000000
"TaskbarGlomLevel"=dword:00000002

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\HideDesktopIcons\NewStartPanel]
"{20D04FE0-3AEA-1069-A2D8-08002B30309D}"=dword:00000000
"{5399E694-6CE5-4D6C-8FCE-1D8870FDCBA0}"=dword:00000000
"{59031a47-3f72-44a7-89c5-5595fe6b30ee}"=dword:00000000
"{645FF040-5081-101B-9F08-00AA002F954E}"=dword:00000000
"{F02C1A0D-BE21-4350-88B0-7367FC96EF3C}"=dword:00000000
"@ | Out-File "$($provisioning.FullName)\user-settings.reg" -Encoding unicode -Force

$configure_active_setup = @{
    Path        = "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\UserSettings"
    ErrorAction = "SilentlyContinue"
}

$configure_runonce = @{
    Name  = "StubPath"
    Value = 'REG IMPORT "{0}"' -f "$($provisioning.FullName)\user-settings.reg"
}

ni @configure_active_setup | New-ItemProperty @configure_runonce
```