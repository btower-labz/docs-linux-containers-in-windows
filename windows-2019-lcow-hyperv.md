# [Linux Containers in Windows](README.md)

## Windows Server 2019 LTS + LOW + HyperV isolation

Nested virtualization support is required to get this solution work.

See options here: [nested-virtualization-links.md](nested-virtualization-links.md)

### Architecture

- Hyper-V is used to run LinuxKit
- All the linux containers are executed inside LinuxKit VM
- Windows containers can be run in both process and hyperv isolation modes

### Windows version selection

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

Windows Server 2019 (version 1809) Standart\Datacenter is the best choice due to LTS and stability.

### Server configuration

Enable Hyper-V components ...

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeAllSubFeature -IncludeManagementTools
Restart-Computer -Force
```

Enable windows Container components ...

```powershell
Install-WindowsFeature Containers
Restart-Computer -Force
```

Enable docker ...


```powershell
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force -Confirm:$False
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force -Scope AllUsers -Confirm:$False
Install-Package -Name docker -ProviderName DockerMsftProvider -Confirm:$False -Force
Restart-Computer -Force
```

Enable docker experimental features ...

```powershell
Set-Content -Value "`{`"experimental`":true`}" -Path C:\ProgramData\docker\config\daemon.json
Restart-Service docker
```

Verify that LCOW is enabled ...

```powershell
docker version
docker info
```

Look for LCOW in command outputs ...

```plain
...
Storage Driver: windowsfilter (windows) lcow (linux)
...
```

Provide docker with LinuxKit packages ...

```powershell
mkdir "C:\Program Files\Linux Containers"
cd "C:\Program Files\Linux Containers"
curl -OutFile release.zip https://github.com/linuxkit/lcow/releases/download/v4.14.35-v0.3.9/release.zip
Expand-Archive -DestinationPath . .\release.zip
rm release.zip
```

Test linux containers ...

```powershell
docker run --interactive --tty ubuntu uname -a
```

Test linux container port exports

```powershell
docker run --detach --publish 80:80 --name webserver nginx
curl http://127.0.0.1:80
docker container kill webserver
```

Test windows containers ...

```powershell
docker run --platform=windows --interactive --isolation=process --tty mcr.microsoft.com/powershell:lts-nanoserver-1809 pwsh.exe -Command {hostname}
```

Test windows container port exports ...

```powershell
docker run -d -p 8080:80 --isolation=hyperv --name iisweb mcr.microsoft.com/windows/servercore/iis
curl http://127.0.0.1:8080
docker container kill iisweb
```

### Docker compose

Enable docker compose ...

```powershell
$url = "https://github.com/docker/compose/releases/download/1.26.1/docker-compose-Windows-x86_64.exe"
Invoke-WebRequest $url -UseBasicParsing -OutFile $Env:ProgramFiles\Docker\docker-compose.exe
```

Clone sample app repository ...

```powershell
docker run -ti --rm -v ${HOME}:/root -v "$((Resolve-Path .\).Path):/git" alpine/git clone https://github.com/btower-labz/docker-compose-sample-app.git
```

Execute and test sample app ...

```powershell
cd docker-compose-sample-app
docker-compose -f app.yml up -d
docker-compose -f app.yml ps
docker container ls
curl http://127.0.0.1:5000
docker-compose -f app.yml kill
```

### References

See: [DockerCon: Linux Containers on Windows - The Inside Story](https://www.youtube.com/watch?v=JZtQnYaO874)

See: [https://github.com/linuxkit/lcow](https://github.com/linuxkit/lcow)

See: [https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/linux-containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/linux-containers)

See: [https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/hyperv-container](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/hyperv-container)

See: [https://success.docker.com/article/how-to-enable-linux-containers-on-windows-server-2019](https://success.docker.com/article/how-to-enable-linux-containers-on-windows-server-2019)

See: [https://docs.docker.com/compose/gettingstarted](https://docs.docker.com/compose/gettingstarted)

See: [https://stefanscherer.github.io/sneak-peek-at-lcow](https://stefanscherer.github.io/sneak-peek-at-lcow)
