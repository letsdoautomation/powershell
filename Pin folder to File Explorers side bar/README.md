```powershell
# MODIFY
$quick_access_name = "Tools 3"
$path = "%USERPROFILE%\Desktop\Tools 3"
$icon = "%systemroot%\system32\shell32.dll,25"

# DO NOT MODIFY
$guid = [guid]::NewGuid().guid

$settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Classes\CLSID\{$($guid)}"
    Value = $quick_access_name
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}"
  Name = "System.IsPinnedToNamespaceTree"
  Value = 1
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}"
  Name = "SortOrderIndex"
  Value = 66
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}\ShellFolder"
  Name = "FolderValueFlags"
  Value = 40
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\HideDesktopIcons\NewStartPanel"
  Name = "{$($guid)}"
  Value = 1
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace\{$($guid)}"
  Value = $quick_access_name
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}\DefaultIcon"
  Value = $icon
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}\Instance"
  Name = "CLSID"
  Value = "{0E5AAE11-A475-4c5b-AB00-C66DE400274E}"
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}\InProcServer32"
  Value = "%systemroot%\system32\shell32.dll"
  Type = [Microsoft.Win32.RegistryValueKind]::ExpandString
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}\Instance\InitPropertyBag"
  Name = "TargetFolderPath"
  Value = $path
  Type = [Microsoft.Win32.RegistryValueKind]::ExpandString
},
[PSCustomObject]@{
  Path  = "SOFTWARE\Classes\CLSID\{$($guid)}\Instance\InitPropertyBag"
  Name = "Attributes"
  Value = 17
} | group Path


foreach($setting in $settings){
  $registry = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey($setting.Name, $true)
  if ($null -eq $registry) {
      $registry = [Microsoft.Win32.Registry]::CurrentUser.CreateSubKey($setting.Name, $true)
  }
  $setting.Group | %{
    if(!$_.Type){
      $registry.SetValue($_.name, $_.value)
    }
    else{
      $registry.SetValue($_.name, $_.value, $_.type)
    }
  }
  $registry.Dispose()
}

Set-ItemProperty -Path "HKCU:\SOFTWARE\Classes\CLSID\{$($guid)}\ShellFolder" -Name "Attributes" -Value 4034920525 -type "DWord"
```