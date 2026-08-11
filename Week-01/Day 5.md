# Day 5 Sunday 8/9/26

Today we built upon Active Directory by:

- [x] Creating Organizational Units (OUs)
  - Servers-Lab
  - Computers-Lab
  - HR
  - IT
  - Sales
  - Security Groups

- [x] Creating Test Users for the Departments
  - HR > hr_user1 & hr_user2
  - IT > it_user1 & it_user2
  - Sales > sales_user1 & sales_user2

- [x] Creating Global Security Groups
  - Security Groups > GG_HR, GG_IT, GG_Sales

- [x] Assigning the test users as members to the correct security group by department
  - HR users > GG_HR
  - IT users > GG_IT
  - Sales users > GG_Sales

- [x] Verifying correct users are members of the correct security groups using PowerShell:
  - `Get-ADUser hr_user1 -Properties MemberOf`
  - `Get-ADUser it_user1 -Properties MemberOf`
  - `Get-ADUser sales_user1 -Properties MemberOf`
  - `Get-ADGroupMember GG_IT`

## Key Takeaways

- OUs are used to organize and manage Active Directory objects such as users, computers, and groups.
- Security groups are used to assign permissions and control access to resources.
- Global Security Groups were created for each department and users were assigned to the appropriate group.
- PowerShell can be used to verify Active Directory users and group membership.
- Group-based permissions are more scalable and easier to manage than assigning permissions to individual users.

<img width="752" height="464" alt="OU Structure" src="https://github.com/user-attachments/assets/7ca7432f-1710-463a-8f39-c96e117255fa" />
<img width="656" height="725" alt="Member Of" src="https://github.com/user-attachments/assets/23d0bb2e-fd1d-4bbc-bcd4-bab20b2ee16a" />
<img width="574" height="316" alt="AD Group Member" src="https://github.com/user-attachments/assets/90ac9bf7-6792-4f59-973d-645f1b3ad40e" />
