# PowerShell: Configure Microsoft Edge KIOSK

<b>Documentation:</b>

* [AssignedAccess (Windows Configuration Designer reference)](https://learn.microsoft.com/en-us/windows/configuration/wcd/wcd-assignedaccess#assignedaccesssettings)
* [Create an Assigned Access configuration XML file](https://learn.microsoft.com/en-us/windows/configuration/assigned-access/configuration-file?pivots=windows-11)
* [Configure Microsoft Edge kiosk mode](https://learn.microsoft.com/en-us/deployedge/microsoft-edge-configure-kiosk-mode)
* [Configure a restricted user experience (multi-app kiosk) with Assigned Access](https://learn.microsoft.com/en-us/windows/configuration/assigned-access/configure-multi-app-kiosk?tabs=ppkg)

<b>Downloads:</b>

* [PsExec](https://learn.microsoft.com/en-us/sysinternals/downloads/psexec)

<b>Run PowerShell under SYSTEM user:</b>

```batch
PsExec.exe -i -s powershell
```

<b>Apply KIOSK configuration:</b>

```powershell
$kiosk_configuration = 
@"
<?xml version="1.0" encoding="utf-8"?>
<AssignedAccessConfiguration xmlns="http://schemas.microsoft.com/AssignedAccess/2017/config" xmlns:rs5="http://schemas.microsoft.com/AssignedAccess/201810/config" xmlns:v4="http://schemas.microsoft.com/AssignedAccess/2021/config">
  <Profiles>
    <Profile Id="{EDB3036B-780D-487D-A375-69369D8A8F78}">
      <KioskModeApp v4:ClassicAppPath="%ProgramFiles(x86)%\Microsoft\Edge\Application\msedge.exe" v4:ClassicAppArguments="--kiosk https://start.duckduckgo.com/ --edge-kiosk-type=public-browsing -kiosk-idle-timeout-minutes=0 --no-first-run" />
    </Profile>
  </Profiles>
  <Configs>
    <Config>
      <AutoLogonAccount rs5:DisplayName="Kiosk" />
      <DefaultProfile Id="{EDB3036B-780D-487D-A375-69369D8A8F78}" />
    </Config>
  </Configs>
</AssignedAccessConfiguration>
"@

# Configure KIOSK mode
$get_cim_instance = @{
  Namespace = "root\cimv2\mdm\dmmap"
  ClassName = "MDM_AssignedAccess"
}
$cim_instance = Get-CimInstance @get_cim_instance
$cim_instance.Configuration = [System.Net.WebUtility]::HtmlEncode($kiosk_configuration)
Set-CimInstance -CimInstance $cim_instance
```

<b>Remove KIOSK configuration:</b>

```powershell
$get_cim_instance = @{
    Namespace = "root\cimv2\mdm\dmmap"
    ClassName = "MDM_AssignedAccess"
}

$cim_instance = Get-CimInstance @get_cim_instance
$cim_instance.Configuration = $null
Set-CimInstance -CimInstance $cim_instance
```