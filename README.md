# Linux Containers in Windows

## Overview

The goal of this investigation is to find out a way to run Linux containers inside the Windows host.

Context: 

- Servers provisioned on-permise. No split or additional provisioning possible in meantime.
- No cloud landing zones available.
- The workload is Windows with some Linux containers.
- Docker compose is required

Beter options:

- It's much better to use compute resources to run Linux containers.
- It's even better to use container orchestration.
- It's much better to use cloud resources due to compute elasticity.

## Available run options

The only **reliable** (huh?) way to run Linux workload on Windows hosts is to use virtualization and publish docker api to the host.

Virtualization software in scope:

- HyperV
- QEMU

vmWare, VirtualBox usage should be also possible.

## Nested virtualization

Nested virtualization support is required to use HyperV container isolation.

In case nested virtualization is not available, it's possible to use QEMU with full software virtualization.

Nested virtualization has less performance impact. With pure software virtualization the performance hit is significant.

See: [nested-virtualization-links.md](nested-virtualization-links.md)

## Windows specifics

Windows support Linux containers with LCOW using HyperV virtualization since Windows Server 1703. This functionality is more or less mature but it's not going to be developed anymore.

Windows support Linux containers with WSL2 using HyperV virtualization since windows Server 2040. This functionality is very new and not tested much.

Both are **EXPERIMENTAL**. Both requires nested virtualization support.

## Virtualization configuration

Option 1: [Windows Server 2019 LTS + LCOW + HyperV isolation](windows-2019-lcow-hyperv.md)

Option 2: Windows Server 2016 LTS + QEMU + Fedora CoreOS

Option 3: Windows Server 2004 SAC + WSL2 + Docker Desktop
