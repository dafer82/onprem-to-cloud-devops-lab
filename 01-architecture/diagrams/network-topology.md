# Network topology (NAT + Host-Only)

```mermaid
flowchart LR
  Internet((Internet))

  Host[Windows 11 Pro (Host)]
  VB[VirtualBox]

  NAT[NAT\n(Outbound Internet)]
  HO[vboxnet0 Host-Only\n192.168.56.1/24\nDHCP: OFF]

  AD[SRV-AD-01\nHost-Only: 192.168.56.10\nRoles: AD DS + DNS]
  LNX[SRV-LNX-01\nHost-Only: 192.168.56.20\nDebian + Docker]

  Host --> VB
  VB --> AD
  VB --> LNX

  AD --> NAT --> Internet
  LNX --> NAT

  AD --- HO
  LNX --- HO
  Host --- HO



