# Preflight and Safety Record

This record captures the decisions made before building the Windows support lab.

## Verified design

| Item | Decision |
|---|---|
| Lab network | Existing OPNsense LAB interface on `10.10.3.0/24` |
| Gateway | OPNsense at `10.10.3.1` |
| Proxmox bridge | `vmbr3` |
| Domain controller | `LAB-DC01`, VM 110, static `10.10.3.20` |
| Workstation | `LAB-PC01`, VM 111, static `10.10.3.30` |
| Domain | `lab.home.arpa` / `LAB` |
| Backup destination | `pve-backup` using snapshot mode and ZSTD compression |

## Guardrails used

- The virtual LAB bridge is separate from the home LAN.
- No Active Directory, DNS, SMB, or Proxmox management service was exposed publicly.
- Only fictional lab users were created.
- Windows evaluation ISOs and VirtIO drivers were used for installation only and are excluded from Git.
- AD/DNS and workstation functionality were verified before access-control and Group Policy work.
- A post-build backup was completed after the environment was validated.

## Resource profile

| VM | vCPU | RAM | Disk | Network |
|---|---:|---:|---:|---|
| `LAB-DC01` | 2 | 4 GB | 80 GB | `vmbr3` |
| `LAB-PC01` | 2 | 4 GB | 64 GB | `vmbr3` |
