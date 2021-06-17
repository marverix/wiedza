# Windows

## BAT

### Dodawanie PATH tymczasowo

```cmd
set PATH=c:\sciezka;%PATH%
```

## PowerShell

### Escapowanie cudzysłowia

```powershell
`"
```

### Włączenie konta

```powershell
Enable-LocalUser -Name "Administrator"
```

albo

```powershell
Get-LocalUser -Name "Administrator" | Enable-LocalUser
```

### Tworzenie usług

Do tworzenia usług używane jest [NSSM](https://nssm.cc/usage)

```powershell
$serviceName = 'service-name'
$serviceDir = 'c:\service-home'

nssm install $serviceName 'c:\absolute\path\to\service.exe'
nssm set $serviceName AppParameters "-some -additional -params"
nssm set $serviceName AppDirectory $serviceDir
nssm set $serviceName AppExit Default Restart
nssm set $serviceName AppStdout "$serviceDir\$serviceName.log"
nssm set $serviceName AppStderr "$serviceDir\$serviceName.error.log"
nssm set $serviceName AppRotateFiles 1
nssm set $serviceName Description 'Service description'
nssm set $serviceName DisplayName 'Service Display Name'
nssm set $serviceName ObjectName LocalSystem
nssm set $serviceName Start SERVICE_AUTO_START
nssm set $serviceName Type SERVICE_WIN32_OWN_PROCESS
```

### Zmień hasło

```powershell
$SecurePassword = ConvertTo-SecureString "here_passoword" -AsPlainText -Force
$UserAccount = Get-LocalUser -Name "username"
$UserAccount | Set-LocalUser -Password $SecurePassword
```

### Acl

Tutaj bardzo dobry art: https://blue42.net/windows/changing-ntfs-security-permissions-using-powershell/

#### Chown

```powershell
Function Chown {
    Param ($_userName, $_path)

    $user = New-Object System.Security.Principal.NTAccount("$_userName")
    $acl = Get-Acl -Path $_path

    # Changing owner
    $acl.SetOwner($user)
    
    # Adding FullControl
    $rule = New-Object System.Security.AccessControl.FileSystemAccessRule($user, "FullControl", "ContainerInherit,ObjectInherit", "None", "Allow")
    $acl.AddAccessRule($rule)
    
    Set-Acl -Path $_path -AclObject $acl
}
```
