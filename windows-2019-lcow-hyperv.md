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
docker run --interactive --tty ubuntu id
```

```powershell
docker run --detach --publish 80:80 --name webserver nginx
curl http://127.0.0.1:80
docker container kill webserver
```

```powershell
docker run --platform=windows --interactive --isolation=process --tty mcr.microsoft.com/powershell:lts-nanoserver-1809 pwsh.exe

```powershell
(Get-ItemProperty -Path c:\windows\system32\hal.dll).VersionInfo.FileVersion
exit
```

```powershell
docker run -d -p 8080:80 --isolation=hyperv --name iisweb mcr.microsoft.com/windows/servercore/iisdocker
curl http://127.0.0.1:8080
```

### Docker compose

```powershell
$url = "https://github.com/docker/compose/releases/download/1.26.1/docker-compose-Windows-x86_64.exe"
Invoke-WebRequest $url -UseBasicParsing -OutFile $Env:ProgramFiles\Docker\docker-compose.exe
```

```
mkdir compose
cd compose
notepad app.yml
```

```yaml
version: '2.1'
services:
  web:
    build:
      context: .
      dockerfile: app.Dockerfile
    ports:
    - "5000:5000"
    volumes:
    - .:/code
    - logvolume01:/var/log
    links:
    - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

```
notepad app.py
```


```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)


def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)


@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

```powershell
notepad requirements.txt
```

```plain
flask
redis
```

```powershell
notepad app.Dockerfile
```

```Dockerfile
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
COPY . .
CMD ["flask", "run"]
```

```powershell
docker-compose -f app.yml up -d
docker-compose -f app.yml ps
docker container ls
curl http://127.0.0.1:5000
docker-compose -f app.yml kill
```

<script src="https://gist.github.com/benstr/8744304.js"></script>

