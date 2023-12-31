# PowerShell: Set Google Chrome as Default browser

<b>Objectives:</b>

* Set Google Chrome as Default browser

<b>Notes:</b> Works for Windows Pro and Enterprise <br />

<b>PowerShell snippet:</b>

```powershell
[System.IO.FileInfo]$DefaultAssociationsConfiguration = "$($env:ProgramData)\provisioning\DefaultAssociationsConfiguration.xml"

if(!$DefaultAssociationsConfiguration.Directory.Exists){
    $DefaultAssociationsConfiguration.Directory.Create()
}

'<?xml version="1.0" encoding="UTF-8"?>
<DefaultAssociations>
  <Association Identifier=".htm" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
  <Association Identifier=".html" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
  <Association Identifier="http" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
  <Association Identifier="https" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
</DefaultAssociations>' | Out-File $DefaultAssociationsConfiguration.FullName -Encoding utf8 -Force

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Windows\System"
    Value = $DefaultAssociationsConfiguration.FullName
    Name  = "DefaultAssociationsConfiguration"
} | group Path

foreach($setting in $settings){
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | %{
        $registry.SetValue($_.name, $_.value)
    }
    $registry.Dispose()
}
```