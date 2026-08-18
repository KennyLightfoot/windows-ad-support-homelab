# Build and Verification Record

## 1. Network and virtualization

The existing OPNsense LAB interface on `vmbr3` was used rather than creating another bridge or changing the home network. The isolated subnet is `10.10.3.0/24`, with OPNsense at `10.10.3.1`.

Both Windows VMs use VirtIO storage and network drivers, OVMF/UEFI, TPM 2.0, and QEMU Guest Agent. Proxmox successfully queried guest operating-system information for both systems after the guest tools installation.

## 2. Domain controller

`LAB-DC01` was installed from a Windows Server 2025 Evaluation ISO and configured with:

```text
Address: 10.10.3.20/24
Gateway: 10.10.3.1
Domain:  lab.home.arpa
NetBIOS: LAB
Roles:   AD DS, DNS, Group Policy Management
```

The domain controller was promoted as the first server in a new forest. DNS was changed from the temporary public resolver to `10.10.3.20` so AD clients resolve through the domain DNS service.

Verification included:

```powershell
Get-ADDomain | Select-Object DNSRoot,NetBIOSName,DomainMode
Get-Service NTDS,DNS | Select-Object Name,Status
nslookup lab.home.arpa 10.10.3.20
nslookup microsoft.com 10.10.3.20
```

## 3. Directory structure and access control

The following organizational units were created at the domain root:

```text
Admins
Disabled Objects
Groups
Servers
Service Accounts
Users
Workstations
```

The initial global security groups are `GG-Helpdesk`, `GG-Accounting`, `GG-Operations`, and `GG-Management`. Fictional employee accounts were placed in the appropriate groups. The Windows 11 computer object was moved from the default Computers container to the Workstations OU after a successful domain join.

## 4. Domain workstation

`LAB-PC01` was configured with a static address and AD DNS:

```text
Address: 10.10.3.30/24
Gateway: 10.10.3.1
DNS:     10.10.3.20
```

Validation from the workstation included domain-controller discovery, LDAP reachability, and domain membership:

```powershell
nltest /dsgetdc:lab.home.arpa
Test-NetConnection 10.10.3.20 -Port 389
Get-CimInstance Win32_ComputerSystem | Select-Object Name,Domain,PartOfDomain
```

The final membership result was `LAB-PC01`, `lab.home.arpa`, and `PartOfDomain: True`. A standard fictional user successfully signed in as `LAB\arivera`.

## 5. SMB permission test

For lab convenience, a test SMB share was hosted on the domain controller:

```text
Path:  C:\LabShares\Helpdesk
Share: \\LAB-DC01\Helpdesk
```

Both share and NTFS permissions use groups rather than individual users:

| Principal | Permission |
|---|---|
| `GG-Helpdesk` | Change/Read at the share; Modify on NTFS |
| `Domain Admins` | Full Control |
| `SYSTEM` / local Administrators | Full Control |

Validation: a Helpdesk user created a test file successfully, while an Accounting user received Access Denied. In production, a dedicated file server would normally host business shares rather than a domain controller.

## 6. Workstation Group Policy

A GPO named `GPO-Workstations-Login-Notice` was linked to the Workstations OU. It configures the following Computer Configuration security options:

```text
Interactive logon: Message title for users attempting to log on
Interactive logon: Message text for users attempting to log on
```

The configured notice was verified on `LAB-PC01` after restart.

## 7. Backup and recovery posture

Snapshot-mode ZSTD backups for VM 110 and VM 111 were completed successfully on `pve-backup`.

An attempted backup to the smaller `local` storage failed due to insufficient capacity. The failed VM 110 archive was removed, the successful VM 111 archive was retained until replacement backup verification, and backup jobs were rerun successfully to `pve-backup`. This was a useful operational lesson: select the correct storage target and verify each job before relying on it.
