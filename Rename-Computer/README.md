# PowerShell: Rename-Computer

<b>Documentation:</b>

* [Rename-Computer](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/rename-computer?view=powershell-5.1)

<b>Get current computer name:</b>

```powershell
$env:COMPUTERNAME
```

<b>Rename computer:</b>

```powershell
Rename-Computer -NewName "computer01"
```

<b>Rename and restart:</b>

```powershell
Rename-Computer -NewName "computer01" -Restart
```

<b>Set computer name from user input:</b>

```powershell
Add-Type -AssemblyName Microsoft.VisualBasic

$computer_name = [Microsoft.VisualBasic.Interaction]::InputBox("Enter new computer name:", "Configure computername")

if(![string]::IsNullOrEmpty($computer_name)){
    Rename-Computer -NewName $computer_name
}
```

<b>Use computer serial number as computer name:</b>

```powershell
# For OEM computers like Dell, Lenovo, HP, etc only

$serial = "PC-{0}" -f ((gwmi -class win32_bios).SerialNumber -replace " ")

if($serial.length -gt 15){
    $serial = $serial.SubString(0, 14)
}

Rename-Computer -NewName $serial
```