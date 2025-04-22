# PowerShell: Configure HKLM Run and RunOnce

<b>Documentation:</b>

* [Run and RunOnce Registry Keys](https://learn.microsoft.com/en-us/windows/win32/setupapi/run-and-runonce-registry-keys)

<b>Run vs RunOnce in HKLM hive</b>

* Process that is executed from RunOnce will be <b>elevated</b> and process executed using Run will <b>not be elavated</b>
* Commands in RunOnce will be executed <b>only</b> for administrator accounts
* Registry entry in RunOnce will be deleted after execution
* Commands in Run will be executed for everyone

<b>Registry locations</b>

* HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
* HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

<b>Configure Run and RunOnce:</b>

```powershell
$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
    Name  = "execute_notepad_from_run"
    Value = "C:\Windows\notepad.exe"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce"
    Name  = "execute_notepad_from_runonce"
    Value = "C:\Windows\notepad.exe"
}

foreach ($setting in ($settings | group Path)) {
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | % {
        if (!$_.Type) {
            $registry.SetValue($_.name, $_.value)
        }
        else {
            $registry.SetValue($_.name, $_.value, $_.type)
        }
    }
    $registry.Dispose()
}
```

<b>Configure RunOnce to execute powershell script:</b>

```powershell
$settings =
[PSCustomObject]@{
    Path  = "SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce"
    Name  = "execute_script"
    Value = "powershell.exe -ExecutionPolicy Bypass -File C:\script.ps1"
}

foreach ($setting in ($settings | group Path)) {
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | % {
        if (!$_.Type) {
            $registry.SetValue($_.name, $_.value)
        }
        else {
            $registry.SetValue($_.name, $_.value, $_.type)
        }
    }
    $registry.Dispose()
}
```

<b>Remove specific registry entries:</b>

```powershell
$remove_registry_entries =
[PSCustomObject]@{
    Path = "SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
    Name = "execute_notepad_from_run"
}

foreach ($entry in ($remove_registry_entries | Group Path)) {
    $key = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($entry.name, $true)
    if ($null -eq $key) {
        continue
    }
    foreach ($item in $entry.Group.Where({ $null -ne $key.GetValue($_.name) })) {
        $key.DeleteValue($item.name, $true)
    }
    $key.Dispose()
}
```

<b>Remove all registry entries:</b>

```powershell
$remove_registry_entries =
[PSCustomObject]@{
    Path = "SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
},
[PSCustomObject]@{
    Path = "SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce"
}

foreach($path in $remove_registry_entries.Path){
    $key = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($path, $true)
    foreach($entry in $key.GetValueNames()){
        $key.DeleteValue($entry, $true)
    }
    $key.Dispose()
}
```