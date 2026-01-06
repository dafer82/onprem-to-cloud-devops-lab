# Virtualization overview

``` mermaid
flowchart TB
    Host[Windows 11 Pro -- Host OS]

    VB[VirtualBox -- Type 2 Hypervisor]

    AD[SRV-AD-01 -- Windows Server 2022]
    LNX[SRV-LNX-01 -- Debian Linux]

    Host --> VB
    VB --> AD
    VB --> LNX
```