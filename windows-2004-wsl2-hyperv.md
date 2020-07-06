# [Linux Containers in Windows](README.md)

## Windows Server 2004 SAC + WSL2 + HyperV isolation

Nested virtualization support is required to get this solution work.

See options here: [nested-virtualization-links.md](nested-virtualization-links.md)

### Architecture

- Hyper-V is used to run WSL2 Ubuntu
- Docker Desktop is used to manage everything

### Windows version selection

LCOW feature is supported from Windows Server 1709 till 2004.

| OS Version                   | OS CodeBase         | Update channel | LCOW Support | End of support |
| ---------------------------- | ------------------- | -------------- | ------------ | -------------- |
| Windows Server, version 2004 | Windows Server 2019 | SAC            | WSL2         | 12/14/2021     |

The only version supported is Windows Server 2004 SAC

### Server configuration

...

### References

[Docker dDsktop and WSL2 history](https://www.youtube.com/watch?v=FD3UVCipmmE)



