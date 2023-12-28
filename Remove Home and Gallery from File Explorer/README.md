# PowerShell: Remove Home and Gallery from File Explorer

<b>Objectives:</b>

* Remove Home and Gallery from File Explorer

<b>Remove home and gallery:</b>

```powershell
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace_41040327\{e88865ea-0e1c-4e20-9aa6-edcd0212c87c}",
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace_36354489\{f874310e-b6b7-47dc-bc84-b9e6b38f5903}" | %{
    ri $_ -force
}
```

<b>CMD remove gallery:</b>

```batch
reg delete HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace_41040327\{e88865ea-0e1c-4e20-9aa6-edcd0212c87c} /f
```

<b>CMD add gallery:</b>

```batch
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace_41040327\{e88865ea-0e1c-4e20-9aa6-edcd0212c87c} /d {e88865ea-0e1c-4e20-9aa6-edcd0212c87c}
```

<b>CMD remove home:</b>

```batch
reg delete HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace_36354489\{f874310e-b6b7-47dc-bc84-b9e6b38f5903} /f
```

<b>CMD add home:</b>

```batch
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace_36354489\{f874310e-b6b7-47dc-bc84-b9e6b38f5903} /d CLSID_MSGraphHomeFolder
```