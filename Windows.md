# Windows

## BAT

### Dodawanie PATH tymczasowo

```cmd
set PATH=c:\sciezka;%PATH%
```

## PowerShell

### Escapowanie cudzysłowia

```powershell
\`"
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
