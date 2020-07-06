# See: https://trevorsullivan.net/2016/06/29/hyper-v-secureboot-error/

https://releases.ubuntu.com/20.04/ubuntu-20.04-live-server-amd64.iso?_ga=2.119765582.773950521.1593784421-798786714.1593784421

$url = "https://releases.ubuntu.com/20.04/ubuntu-20.04-live-server-amd64.iso?_ga=2.119765582.773950521.1593784421-798786714.1593784421"
$output = "$PSScriptRoot\ubuntu-20.04-live-server-amd64.iso"
$start_time = Get-Date

$url = "https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/32.20200615.3.0/x86_64/fedora-coreos-32.20200615.3.0-live.x86_64.iso"
$output = "/fedora-coreos-32.20200615.3.0-live.x86_64.iso"


$url = "https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/32.20200615.3.0/x86_64/fedora-coreos-32.20200615.3.0-azure.x86_64.vhd.xz"
$output = "/fedora-coreos-32.20200615.3.0-azure.x86_64.vhd.xz"

$url = "https://github.com/libarchive/libarchive/releases/download/v3.4.3/libarchive-v3.4.3-win64.zip"
$output = "/libarchive-v3.4.3-win64.zip"

[Net.ServicePointManager]::SecurityProtocol = "Tls12, Tls11, Tls, Ssl3"
Import-Module BitsTransfer
Start-BitsTransfer -Source $url -Destination $output

Install-Module -Name 7Zip4Powershell
Import-Module -Name 7Zip4Powershell -Global


Write-Output "Time taken: $((Get-Date).Subtract($start_time).Seconds) second(s)"

New-VM -Name "UbuntuLinux" -Generation 2 -SwitchName "HVInternal"

New-VM -Name "UbuntuLinux" --SwitchName

-NewVHDPath <VHDPath>`

NewVHDSizeBytes <Memory> `

-Generation <Generation> `

-MemoryStartupBytes <Memory> `

-SwitchName <SwitchName> `

Set-VM -Name <Name> `

-ProcessorCount <Number of Processors> `

-DynamicMemory `

-MemoryMinimumBytes <Memory> `

-MemoryStartupBytes <Memory> `

-MemoryMaximumBytes <Memory> `
