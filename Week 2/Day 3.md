# Day 8 - Wednesday 8/12/26

## PowerShell Automation & AD User Lifecycle

Day 8 focused on administering Active Directory instead of building the Active Directory environment.

* I started by checking the environment I currently built by doing an inventory check on Users, Groups, Computers, and Organizational Units
  * `Get-ADUser -Filter *`
  * `Get-ADGroup -Filter *`
  * `Get-ADComputer -Filter *`
  * `Get-ADOrganizationalUnit -Filter *`
* I then practiced searching for more specific information
  * Find HR users: `Get-ADUser -Filter 'SamAccountName -like "hr_*"' | Select Name,SamAccountName,Enabled`
  * Find disabled users: `Search-ADAccount -UsersOnly -AccountDisabled`
  * Find enabled users: `Search-ADAccount -UsersOnly -AccountActive` (see Troubleshooting below)

## Troubleshooting

I received an error with the last command searching for active accounts:

`Search-ADAccount -UsersOnly -AccountActive`

I used `man Search-ADAccount` to review the available syntax and found that `-AccountActive` was not a valid parameter. The available account-status parameters included `-AccountDisabled`, `-AccountExpired`, `-AccountExpiring`, `-AccountInactive`, `-LockedOut`, `-PasswordExpired`, and `-PasswordNeverExpires`.

I then found an alternative method of searching for enabled users:

`Get-ADUser -Filter 'Enabled -eq $true'`

This was a good reminder to verify PowerShell syntax and investigate an error rather than assuming the problem is with my environment.

* The last thing I did was an onboarding lab where I created a new user
  * `New-ADUser -Name "HR NewHire1" -SamAccountName hr_newhire1 -UserPrincipalName hr_newhire1@reedlab.local -Path "OU=HR,DC=reedlab,DC=local" -AccountPassword (Read-Host -AsSecureString "Password") -Enabled $true`
* Added the user to the HR Global Group
  * `Add-ADGroupMember GG_HR hr_newhire1`
* Then verified
  * `Get-ADUser hr_newhire1`
  * `Get-ADGroupMember GG_HR`

## What I Learned

* Don't blindly follow a guide because you may come up with an error. Instead of freezing, see if you can find the reason for the error and how to correct it.
* I was usually pretty timid every time I tried using PowerShell in the past, but the more I get on it, the more comfortable I'm becoming.

<img width="744" height="253" alt="Get-ADUser" src="https://github.com/user-attachments/assets/563b8ba2-f9cb-4426-be37-8076032a8454" />
<img width="961" height="576" alt="Man Search-ADAccount" src="https://github.com/user-attachments/assets/e6f8844f-d163-4bbd-989b-2a8274900f16" />
<img width="974" height="356" alt="Onboarding" src="https://github.com/user-attachments/assets/fd41bd84-9a24-47b5-84fa-b9e7d0b726d8" />


