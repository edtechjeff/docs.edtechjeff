\---

title: DHCP

weight: 10

\---



\# Windows Server DHCP



Notes and procedures for managing DHCP on Windows Server.



\## View DHCP Scopes



```powershell

Get-DhcpServerv4Scope

```



\## DHCP Server Backup



```powershell

Backup-DhcpServer -ComputerName "DHCP01" -Path "C:\\DHCPBackup"

```



\## DHCP Server Restore



```powershell

Restore-DhcpServer -ComputerName "DHCP01" -Path "C:\\DHCPBackup" -Force

```

