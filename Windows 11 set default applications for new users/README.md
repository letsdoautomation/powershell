# PowerShell: Windows 11 set default applications for new users

<b>Notes:</b>

* This method works only for <b>NEW</b> users on computer

<b>Objectives:</b>

* Create default app associations xml
* Use app associations xml to:
    * Set Adobe Reader as default app for pdf files
    * Set Chrome as default browser

<b>Export curret default application list:</b>

```powershell
dism /online /Export-DefaultAppAssociations:"C:\associations.xml"
```

<b>Set default programs for new users:</b>

```powershell
$associations_xml = @"
<?xml version="1.0" encoding="UTF-8"?>
<DefaultAssociations>
  <Association Identifier=".htm" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
  <Association Identifier=".html" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
  <Association Identifier=".pdf" ProgId="AcroExch.Document.DC" ApplicationName="Adobe Acrobat Reader" />
  <Association Identifier="http" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
  <Association Identifier="https" ProgId="ChromeHTML" ApplicationName="Google Chrome" />
</DefaultAssociations>
"@

$provisioning = ni "$($env:ProgramData)\provisioning" -ItemType Directory -Force

$associations_xml | Out-File "$($provisioning.FullName)\associations.xml" -Encoding utf8

dism /online /Import-DefaultAppAssociations:"$($provisioning.FullName)\associations.xml"
```