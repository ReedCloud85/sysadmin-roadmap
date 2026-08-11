# Day 4 Saturday 8/8/26

Day 3 was a light refresher on everything that was done on Days 1 and 2 and verifying everything was still functional by checking the hostname using `whoami`, network information using `ipconfig /all`, and external subnet connectivity using `ping`.

## Objectives

- [x] Install AD DS Role
- [x] Create new forest for Active Directory
  - Domain: `reedlab.local`
- [x] Promote `SRV-DC01` to Domain Controller
- [x] Verify functionality of promotion
  - DNS Manager
  - Active Directory Users and Computers
  - `nslookup`

## Environment

- Server: `SRV-DC01`
- OS: Windows Server 2022
- Domain: `reedlab.local`
- Role: Domain Controller
- AD DS: Installed
- DNS: Installed/configured through AD DS promotion
- Global Catalog: Enabled
- DSRM password: Configured and stored securely

## What I Learned

### Active Directory Domain Services

AD DS provides centralized directory services and authentication for the domain. A Domain Controller hosts AD DS and manages directory objects such as users, computers, and groups.

### Active Directory Forest

Because this is a new and isolated lab environment with no existing Active Directory domain, I created a new forest with `reedlab.local` as the forest root domain.

### DNS and Active Directory

Active Directory relies heavily on DNS so domain-joined systems can locate Domain Controllers and other domain services. DNS provides name resolution, while routing determines how traffic reaches the destination.

### Global Catalog

The Global Catalog contains a partial, searchable copy of objects from domains throughout the forest. It allows clients and administrators to search for objects across the forest and supports functions such as universal group membership.

### Directory Services Restore Mode

DSRM provides a specialized recovery mode for performing certain Active Directory recovery and maintenance operations when normal directory services are unavailable. The DSRM password was configured during Domain Controller promotion and is stored securely outside of GitHub.

## Verification

After promotion, I verified the Domain Controller was functioning by:

1. Opening DNS Manager and confirming the DNS zones were present.
2. Opening Active Directory Users and Computers and confirming the domain was available.
3. Using `nslookup` to verify DNS resolution for the domain.
4. Confirming `SRV-DC01` was operating as the Domain Controller for `reedlab.local`.

## Troubleshooting / Lessons Learned

During the Domain Controller promotion process, I initially went through the configuration wizard without stopping to fully understand every option.

This reinforced an important lesson: configuration decisions should not be treated as "click Next until finished." When a wizard presents a setting that changes system behavior, I should understand what the setting does and why it is enabled or disabled before continuing.

This is especially important when configuring infrastructure such as Active Directory, DNS, networking, security, or permissions.

## Key Takeaways

- A Domain Controller provides centralized authentication and directory services.
- Active Directory relies heavily on DNS for service and Domain Controller discovery.
- DNS resolution and network routing are separate functions.
- The Global Catalog provides forest-wide searchable directory information.
- DSRM provides a recovery mechanism for Active Directory.
- Domain Controllers should have predictable IP addressing.
- Infrastructure configuration decisions should be understood rather than blindly accepted.

<img width="756" height="535" alt="Active Directory" src="https://github.com/user-attachments/assets/a1b9d8bf-3db4-443a-83f2-9d0e5cc2dd84" />
<img width="472" height="208" alt="nslookup" src="https://github.com/user-attachments/assets/27c80eb4-5292-404e-bdb1-ddd5949b7317" />
<img width="759" height="532" alt="DNS" src="https://github.com/user-attachments/assets/0f24627c-fe01-408c-ac62-673d95b91965" />
