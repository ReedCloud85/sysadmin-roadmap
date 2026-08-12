# Day 6 - Monday 8/10/26

## Group Policy - Centralized Windows Administration

Day 6 focused on moving from building Active Directory to administering it. The lab involved joining a Windows 11 client to the `reedlab.local` domain, creating and linking a Group Policy Object (GPO), applying the policy, verifying the result, and troubleshooting issues encountered during the process.

## Objectives

* [x] Verify AD/DNS health
* [x] Verify the Windows 11 client
* [x] Join the client to `reedlab.local`
* [x] Move `WIN11-CLIENT01` into `Computers-Lab`
* [x] Create `LAB-Computer-Baseline`
* [x] Link the GPO to `Computers-Lab`
* [x] Configure a visible test policy
* [x] Run `gpupdate /force`
* [x] Verify the applied policy with `gpresult /r`
* [x] Troubleshoot and document issues encountered

## Environment

* Domain Controller: `SRV-DC01`
* Domain: `reedlab.local`
* Windows 11 Client: `WIN11-CLIENT01`
* Domain Controller IP/DNS: `192.168.150.200`
* Client OU: `Computers-Lab`
* GPO: `LAB-Computer-Baseline`

## Active Directory Client Configuration

The Windows 11 client was configured to use `SRV-DC01` (`192.168.150.200`) as its DNS server.

This is necessary because the client needs to resolve the internal `reedlab.local` domain and locate Active Directory services. Public DNS servers such as `8.8.8.8` do not contain the private DNS records used by the lab's Active Directory environment.

## Domain Join

`WIN11-CLIENT01` was successfully joined to:

```text
reedlab.local
```

After joining the domain, the client was restarted and the computer account was located in Active Directory Users and Computers.

The computer account was then moved into:

```text
Computers-Lab
```

This provided a controlled OU in which to test the computer-side Group Policy.

## Group Policy Configuration

A new GPO was created:

```text
LAB-Computer-Baseline
```

The GPO was configured with a visible computer-side policy under:

```text
Computer Configuration
└── Policies
    └── Windows Settings
        └── Security Settings
            └── Local Policies
                └── Security Options
```

The interactive logon message was configured as:

```text
Title: REEDLAB LAB SYSTEM
Message: Authorized lab users only.
```

The GPO was intended to demonstrate the full process of:

```text
Create GPO
    ↓
Link GPO
    ↓
Apply GPO
    ↓
Verify GPO
```

## GPO Scope

The GPO was linked to the `Computers-Lab` OU rather than being intentionally limited to the entire domain.

This provided a controlled scope for testing the policy and reduced the potential impact of a configuration mistake.

Creating a GPO does not automatically apply it. The GPO must be linked to a site, domain, or OU within the scope of the target object.

## Applying and Verifying Group Policy

On `WIN11-CLIENT01`, Group Policy was manually refreshed using:

```powershell
gpupdate /force
```

The resulting policies were then checked using:

```powershell
gpresult /r
```


## Troubleshooting

### Issue 1 - Windows 11 Client Could Not Join the Domain

The initial domain join attempt failed because the Windows 11 client was not correctly using the Domain Controller for DNS.

The client was manually configured to use:

```text
DNS Server: 192.168.150.200
```

This pointed the client to `SRV-DC01`, which hosts the DNS service for the `reedlab.local` Active Directory environment.

After correcting the DNS configuration, the client was able to resolve the internal domain and successfully join `reedlab.local`.

### Resolution

The key troubleshooting lesson was that an Active Directory client should use the Domain Controller's DNS service rather than a public DNS resolver.

The troubleshooting chain was:

```text
Domain Join Failure
        ↓
Check DNS
        ↓
Client was not correctly using DC DNS
        ↓
Point DNS to 192.168.150.200
        ↓
Verify reedlab.local resolution
        ↓
Successfully join domain
```

### Issue 2 - GPO Appeared Twice in Applied Policies

After applying `LAB-Computer-Baseline`, the GPO appeared twice when reviewing the applied Group Policy results.

The issue was traced to the GPO being linked in two locations:

```text
reedlab.local
└── LAB-Computer-Baseline

Computers-Lab
└── LAB-Computer-Baseline
```

Because the GPO was linked both at the domain level and to the `Computers-Lab` OU, the policy appeared twice in the resulting Group Policy information.

### Resolution

The unnecessary duplicate link was identified and removed so that `LAB-Computer-Baseline` was intentionally scoped to `Computers-Lab`.

This reinforced the importance of understanding **GPO scope and linking** rather than assuming that a policy appearing in Group Policy Management automatically means it is being applied in the intended way.

## Computer Configuration vs. User Configuration

Computer Configuration applies settings to the computer, regardless of which user logs in.

User Configuration applies settings to the user, regardless of which domain-joined computer they use.

A useful distinction is:

> Computer Configuration follows the computer.
> User Configuration follows the user.

The scope of the GPO and the type of configuration inside the GPO must match what is being controlled.

## Troubleshooting Approach

When a GPO does not apply, the dependency chain should be checked rather than immediately deleting or recreating the GPO.

Important checks include:

1. Is the client joined to `reedlab.local`?
2. Is the client using `SRV-DC01` for DNS?
3. Can the client resolve `reedlab.local`?
4. Is `WIN11-CLIENT01` located in `Computers-Lab`?
5. Is `LAB-Computer-Baseline` linked to `Computers-Lab`?
6. Is the setting configured under the correct Computer/User Configuration section?
7. Did `gpupdate /force` complete successfully?
8. Does `gpresult /r` show the GPO?
9. Is another GPO overriding or blocking the setting?

## Key Takeaways

* Active Directory clients depend on the Domain Controller's DNS service for internal domain and service resolution.
* A Windows client must be able to resolve the AD domain before it can successfully join the domain.
* Creating a GPO does not automatically apply it; it must be linked to an appropriate scope.
* Computer Configuration and User Configuration affect different types of objects.
* `gpupdate /force` forces Group Policy to be reapplied.
* `gpresult /r` helps verify which policies actually applied.
* GPO linking determines the policy's scope, while security filtering can further control which objects are permitted to apply it.
* Limiting test policies to a controlled OU reduces the potential impact of configuration mistakes.
* Duplicate GPO links can produce confusing results and should be identified when troubleshooting Group Policy.
* Troubleshooting should follow the dependency chain instead of immediately rebuilding the configuration.

<img width="416" height="501" alt="Interactive Logon Title GPO" src="https://github.com/user-attachments/assets/202e2878-8358-4b3e-a063-aea6f5f55e1f" />
<img width="415" height="505" alt="Interactive Logon Message GPO" src="https://github.com/user-attachments/assets/eaa9168d-df58-494d-afe1-0ce92f664c93" />
<img width="659" height="464" alt="Applied Group Policy Objects" src="https://github.com/user-attachments/assets/9282a320-71cd-4fb5-9f77-db4c62a9db51" />
<img width="752" height="524" alt="Group Policy Management" src="https://github.com/user-attachments/assets/f1319754-4f9a-418b-90d2-ec409baeed0c" />
<img width="750" height="525" alt="Delete Domain Link" src="https://github.com/user-attachments/assets/d7ec3695-f4bd-48e6-b745-d7a4378b01f5" />
<img width="650" height="393" alt="Applied Group Policy Objects Revised" src="https://github.com/user-attachments/assets/09d6ebb6-0e71-495e-8e55-22d60d33777e" />
<img width="1021" height="885" alt="GPO Visual Confirmation" src="https://github.com/user-attachments/assets/7d9e56d2-8992-4f74-ba13-10bc9fe4ead8" />

