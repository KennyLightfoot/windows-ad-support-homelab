# Baseline and Client Discovery

This is a simulated small-business client environment. The current baseline reflects the implemented Windows support lab; future scenarios should preserve the same isolation and documentation rules.

## Business discovery

- Business name: Northstar Family Services (fictional)
- Number of initial test users: 4
- Critical operating hours: 8:00 AM-6:00 PM weekdays
- Critical services: sign-in, DNS, internet, shared files, printing
- Primary simulated contact: Office Manager
- Change-approval contact: Owner
- Emergency definition: Multiple users unable to work or suspected security incident

## Technical discovery

Document:

- LAB addressing: OPNsense `10.10.3.1`, domain controller `10.10.3.20`, workstation `10.10.3.30`
- Firewall interface and isolation boundary: OPNsense LAB / `vmbr3`
- DHCP: intentionally not used yet; Windows systems use documented static addresses
- DNS servers and zones
- Servers and services
- User and administrator accounts
- Groups and folder permissions
- Installed applications
- Backup method: Proxmox snapshot-mode ZSTD backups stored on `pve-backup`
- Monitoring targets and alert destinations
- Vendors and escalation contacts (fictional)

## Baseline tests

Record the result and date for each:

- Client has its documented static IP, gateway, and AD DNS configuration
- Client reaches default gateway
- Client resolves the domain controller by name
- Client resolves an external hostname
- Domain user can sign in
- Standard user cannot install software requiring elevation
- Approved shared folder opens
- Unauthorized shared folder is denied
- Future Ubuntu and monitoring services are not part of the current baseline
- Time is synchronized
- Relevant system logs are accessible

Do not begin fault injection until this baseline passes.
