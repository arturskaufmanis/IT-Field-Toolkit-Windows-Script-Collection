Windows 10 / 11 Free to use Run as Admin 14 Scripts

# IT Field Toolkit Windows Script Collection

A portable USB toolkit for IT technicians. Diagnose, repair, and harden Windows standalone machines — no install required, just plug in and run.

Standalone &amp; workgroup machines .bat + PowerShell All logs saved to C:\\Logs\\

## ⚡ Quick Start

1

Download and extract `IT_Field_Toolkit_USB.zip` to the root of a USB drive.

2

On the target Windows PC, open PowerShell as Administrator and run once:

```
Set-ExecutionPolicy RemoteSigned -Scope LocalMachine
```

3

Navigate to the USB drive and run scripts in numbered order, or jump to the relevant category.

4

All output logs are saved automatically to `C:\Logs\` with the hostname and timestamp in the filename.

📁 Folder Structure

IT\_Field\_Toolkit\_USB\\ │ │ README.txt ← run order + setup notes │ ├─ 01\_System\_Health\\ │ 01\_SFC\_DISM\_Repair.bat ← run first on any sick machine │ 02\_EventLog\_Errors.ps1 ← 48h critical/error events → CSV │ 03\_SystemInfo\_Snapshot.bat ← full HW/OS inventory to log │ ├─ 02\_Network\\ │ 04\_Net\_Diagnostics.bat ← IP, DNS, ping, tracert, netstat │ 05\_Net\_Stack\_Reset.bat ← Winsock/TCP reset (prompts reboot) │ 06\_DNS\_Flush\_Renew.bat ← lighter fix: flush + DHCP renew │ ├─ 03\_Performance\\ │ 07\_Temp\_Cleanup.bat ← Temp, Prefetch, WER, WU cache │ 08\_Startup\_Audit.ps1 ← registry + folders + tasks → CSV │ 09\_Disk\_Health\_Check.bat ← SMART status, dirty bit, chkdsk │ 10\_Perf\_Snapshot.ps1 ← CPU/RAM/processes, colour-coded │ └─ 04\_Security\\ 11\_Defender\_Quick\_Scan.bat ← update sigs + quick scan 12\_Firewall\_Status.ps1 ← profiles, open ports, risky flags 13\_Windows\_Update\_Reset.bat ← clears WU cache, restarts services 14\_Security\_Audit.ps1 ← users, shares, ports, UAC, RDP

📋 Scripts

🔧

System Health &amp; Repair

3 scripts

01\_SFC\_DISM\_Repair.bat

Runs DISM to restore the Windows component store, then SFC to repair protected system files. Run this first on any machine with stability issues. Takes 20–40 min.

.bat

02\_EventLog\_Errors.ps1

Pulls all Critical and Error events from System and Application logs in the last 48 hours. Groups by Event ID frequency. Exports to CSV.

.ps1 read-only

03\_SystemInfo\_Snapshot.bat

Full hardware and OS inventory: CPU, RAM, disks, GPU, NICs, hotfixes, running services. Saves to a timestamped log. Good starting point for any new machine.

.bat read-only

🌐

Network Diagnostics &amp; Repair

3 scripts

04\_Net\_Diagnostics.bat

Full network diagnostic sweep: IP config, DNS cache, routing table, ARP, gateway ping, internet ping, DNS resolution test, traceroute, and netstat. Saves timestamped log. Shows a pass/fail summary at the end.

.bat read-only

05\_Net\_Stack\_Reset.bat

Resets Winsock, TCP/IP stack, and Windows Firewall to defaults. Releases and renews DHCP. Fixes most software-level connectivity failures. Prompts before running and offers reboot.

.bat ⚠ reboot

06\_DNS\_Flush\_Renew.bat

Lighter network fix: flushes DNS resolver cache, releases and renews DHCP lease, re-registers DNS. Use this before escalating to the full stack reset.

.bat

⚡

Performance &amp; Cleanup

4 scripts

07\_Temp\_Cleanup.bat

Clears Windows Temp, user Temp, Prefetch, Windows Error Reporting dumps, WU download cache, and thumbnail cache. Does not touch personal files. Reports free space before and after.

.bat

08\_Startup\_Audit.ps1

Lists every startup entry: registry Run keys (HKLM + HKCU), startup folders, scheduled tasks triggered at boot/logon, and non-Microsoft auto-start services. Exports to CSV for review.

.ps1 read-only

09\_Disk\_Health\_Check.bat

Reports SMART status for all physical drives, checks logical disk free space, queries the volume dirty bit, and optionally schedules chkdsk /f /r on C: for next reboot.

.bat

10\_Perf\_Snapshot.ps1

Point-in-time performance reading: CPU load, RAM usage, top 15 processes by CPU and RAM, disk usage per drive with free %, and pagefile current/peak/max. Colour-coded output (green/yellow/red).

.ps1 read-only

🔒

Security Hardening

4 scripts

11\_Defender\_Quick\_Scan.bat

Updates Windows Defender virus definitions, then runs a Quick Scan via MpCmdRun.exe. Reports clean or flags threats for review in Windows Security. Change ScanType 1 to 2 for a full scan.

.bat

12\_Firewall\_Status.ps1

Checks all three firewall profiles (Domain/Private/Public), lists non-system inbound allow rules, flags any disabled profiles in red, checks for outbound blocks, and verifies the firewall service is running.

.ps1 read-only

13\_Windows\_Update\_Reset.bat

Stops WU services, backs up and clears the SoftwareDistribution cache and catroot2, resets BITS, restarts all services, and triggers a new update scan. Fixes stuck, failing, or looping updates.

.bat ⚠ reboot rec.

14\_Security\_Audit.ps1

Read-only security snapshot: local users and admins, non-default shared folders, all listening TCP ports with process names, RDP status, UAC status, Windows Defender health, and AutoRun configuration. Saves full report.

.ps1 read-only

⚙️ Requirements

**⚠ Before running PowerShell scripts**

Windows blocks unsigned scripts by default. Run this once on the target machine in an Admin PowerShell session:

Admin PowerShell — run once per machine

```
Set-ExecutionPolicy RemoteSigned -Scope LocalMachine
```

OS

Windows 10 or Windows 11 (standalone or workgroup — no domain required)

Privileges

All scripts must be run as Administrator (right-click → Run as Administrator)

PowerShell

PowerShell 5.1+ (built-in on all Windows 10/11 machines)

Internet

Required for 01\_SFC\_DISM\_Repair.bat (DISM downloads from Windows Update) and 11\_Defender\_Quick\_Scan.bat (definition updates). All other scripts are fully offline.

Logs

All output is saved to `C:\Logs\` — created automatically if it doesn't exist

📝 Usage Notes

Safe defaults

Scripts tagged *read-only* make no changes to the system — they only collect and display information. All destructive or reset scripts prompt for confirmation first.

Multiple machines

Log filenames include the computer hostname and timestamp, so running scripts across multiple machines won't overwrite each other's logs.

Recommended run order

For a full service, run in numbered order (01 → 14). For a targeted fix, jump directly to the relevant category. Scripts are independent of each other.

Reboot scripts

**05\_Net\_Stack\_Reset.bat** and **13\_Windows\_Update\_Reset.bat** require a reboot to fully take effect. Both offer an optional automatic reboot prompt at the end.

Modifying scripts

All scripts are plain text — open in Notepad or VS Code. Feel free to adapt them to your environment. No proprietary formats or external dependencies.

### 📄 Licence — Free to Use

This toolkit is released free of charge for personal and professional use. You may use, copy, modify, and distribute these scripts freely. No attribution required.

These scripts are provided as-is with no warranty. Always review scripts before running on production systems. The author accepts no liability for data loss or system damage resulting from use.

IT Field Toolkit · Windows Script Collection · Free to use and modify
