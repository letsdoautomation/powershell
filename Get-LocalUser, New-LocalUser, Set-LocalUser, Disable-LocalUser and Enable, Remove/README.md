# PowerShell: Get-LocalUser, New-LocalUser, Set-LocalUser, Disable-LocalUser and Enable, Remove

<b>Documentation:</b>

* [Get-LocalUser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/get-localuser?view=powershell-5.1)
* [New-LocalUser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/new-localuser?view=powershell-5.1)
* [Set-LocalUser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/set-localuser?view=powershell-5.1)
* [Disable-LocalUser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/disable-localuser?view=powershell-5.1)
* [Enable-LocalUser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/enable-localuser?view=powershell-5.1)
* [Remove-LocalUser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/remove-localuser?view=powershell-5.1)

### Get-LocalUser

<b>Get all local user accounts:</b>

```powershell
Get-LocalUser
```

<b>Get all properties:</b>

```powershell
Get-LocalUser | select *
```

<b>Select specific properties:</b>

```powershell
Get-LocalUser | select Name, Enabled, PasswordLastSet
```

<b>Get only Enabled users:</b>

```powershell
Get-LocalUser | ?{$_.Enabled} | select Name, Enabled, PasswordLastSet
```

### New-LocalUser

<b>Create user:</b>

```powershell
New-LocalUser
```

<b>Create user without password and add him to Users group:</b>

```powershell
$new_user = @{
    Name                 = 'bobUser'
    NoPassword           = $true
}
$user = New-LocalUser @new_user
$user | Set-LocalUser -PasswordNeverExpires $true 
$user | Add-LocalGroupMember -Group "Users"
```

<b>Create user without password and add him to Administrators group:</b>

```powershell
$new_user = @{
    Name                 = 'bobAdmin'
    NoPassword           = $true
}
$user = New-LocalUser @new_user
$user | Set-LocalUser -PasswordNeverExpires $true 
$user | Add-LocalGroupMember -Group "Administrators"
```

<b>Create multiple users with password and add them to Administrators group:</b>

```powershell
$new_users = @{
    Name     = 'user01'
    Password = (ConvertTo-SecureString -AsPlainText "Password55" -Force)
},
@{
    Name     = 'user02'
    Password = (ConvertTo-SecureString -AsPlainText "Password55" -Force)
},
@{
    Name     = 'user03'
    Password = (ConvertTo-SecureString -AsPlainText "Password55" -Force)
}

foreach ($new_user in $new_users) {
    $user = New-LocalUser @new_user
    $user | Set-LocalUser -PasswordNeverExpires $true 
    $user | Add-LocalGroupMember -Group "Administrators"
}
```

<b>Create user with passwordand add him to Administrators group:</b>

```powershell
Get-Credential | select @{n = 'Name'; e = { $_.UserName } },
@{n = 'Password'; e = { $_.Password } } | New-LocalUser -PasswordNeverExpires | Add-LocalGroupMember -Group "Administrators"
```

### Disable-LocalUser

<b>Disable single user:</b>

```powershell
Disable-LocalUser user02
```

<b>Disable multiple users:</b>

```powershell
Disable-LocalUser 'user01', 'user02', 'user03'
```

<b>Disable filtered users:</b>

```powershell
Get-LocalUser | ?{$_.Enabled -and $_.Name -ne 'admin'} | Disable-LocalUser
```

### Enable-LocalUser

<b>Enable single user:</b>

```powershell
Enable-LocalUser user01
```

<b>Enable filtered users:</b>

```powershell
Get-LocalUser | ?{!$_.Enabled -and $_.Name -eq 'administrator'} | Enable-LocalUser
```

### Set-LocalUser

<b>Set password for administrator user:</b>

```powershell
Set-LocalUser 'administrator' -Password (ConvertTo-SecureString -AsPlainText "Password55" -Force)
```

### Remove-LocalUser

<b>Remove single user:</b>

```powershell
Remove-LocalUser user02
```

<b>Exclude users from the list and remove everything else</b>

```powershell
$exclude = 
'WDAGUtilityAccount',
'DefaultAccount',
'Administrator',
'Guest',
'admin'

$users = Get-LocalUser

foreach($user in $users.Where({$_.name -notin $exclude})){
    $user | Remove-LocalUser 
}
```