# Linux Containers in Windows

## Windows Server 2019 LTS + LOW + HyperV isolation

### Architecture


### windows version selection

LCOW feature is supported from Windows Server 1709 till 2004.

| OS Version                   | OS CodeBase         | Update channel | LCOW Support | End of support |
| ---------------------------- | ------------------- | -------------- | ------------ | -------------- |
| Windows Server, version 1607 | Windows Server 2016 | LTS            | NOPE         | 01/11/2022     |
| Windows Server, version 1709 | Windows Server 2016 | SAC            | LCOW         | 01/09/2017     |
| Windows Server, version 1803 | Windows Server 2016 | SAC            | LCOW         | 11/10/2020     |
| Windows Server, version 1809 | Windows Server 2019 | SAC            | LCOW         | 11/10/2020     |
| Windows Server, version 1809 | Windows Server 2019 | LTS            | LCOW         | 01/09/2024     |
| Windows Server, version 1903 | Windows Server 2019 | SAC            | LCOW         | 12/08/2020     |
| Windows Server, version 1909 | Windows Server 2019 | SAC            | LCOW         | 05/11/2021     |
| Windows Server, version 2004 | Windows Server 2019 | SAC            | WSL2         | 12/14/2021     |

Windows Server 2019 (version 1809) Standart\Datacenter is the best choice due to LTS.

### Server configuration

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeAllSubFeature -IncludeManagementTools
Restart-Computer -Force
```

```powershell
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force -Confirm:$False
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force -Scope AllUsers -Confirm:$False
Install-Package -Name docker -ProviderName DockerMsftProvider -Confirm:$False -Force
Restart-Computer -Force
```

```powershell
Install-WindowsFeature Containers
Restart-Computer -Force
```

```powershell
Set-Content -Value "`{`"experimental`":true`}" -Path C:\ProgramData\docker\config\daemon.json
Restart-Service docker
```

```powershell
docker version
docker info
```

```plain
...
Storage Driver: windowsfilter (windows) lcow (linux)
...
```

```powershell
mkdir "C:\Program Files\Linux Containers"
cd "C:\Program Files\Linux Containers"
curl -OutFile release.zip https://github.com/linuxkit/lcow/releases/download/v4.14.35-v0.3.9/release.zip
Expand-Archive -DestinationPath . .\release.zip
rm release.zip
```

```powershell
$url = "https://github.com/docker/compose/releases/download/1.26.1/docker-compose-Windows-x86_64.exe"
Invoke-WebRequest $url -UseBasicParsing -OutFile $Env:ProgramFiles\Docker\docker-compose.exe
```
