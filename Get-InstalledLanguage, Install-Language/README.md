# PowerShell: Get-InstalledLanguage, Install-Language

# Show installed languages
```powershell
Get-InstalledLanguage
```

# Install language
```powershell
$install_language = @{
    Language       = "fr-FR"
    CopyToSettings = $true
}
Install-Language @install_language
```

# Install language if needed
```powershell
$language = "fr-FR"
$language_installed = Get-InstalledLanguage $language
if(!$language_installed){
        $install_language = @{
        Language       = $language
        CopyToSettings = $true
    }
    Install-Language @install_language
}
```