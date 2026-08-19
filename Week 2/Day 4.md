# Day 9 - Thursday 8/13/26

## PowerShell Automation & AD User Lifecycle

In today's lab, I focused on creating an automated script to create users while also incorporating error handling with a try/catch method.

* We started with creating variables with the script:
  * $SamAccountName = "hr_newhire2"
  * $DisplayName = "HR NewHire2"
  * $Department = "HR"
 
* Next was putting together the script to create the new hire for the onboarding, for now I used the same script that was used in the previous lab while adding the variables
  *  New-ADUser -Name $DisplayName -SamAccountname $SamAccountName -UserPrincipalName hr_newhire2@reedlab.local -Department $Department -Path "OU=HR,DC=reedlab,DC=local" -AccountPassword (Read-Host -AsSecureString "Password") -Enabled $true

* Next was adding Error Handling
## This step slowed me down as I had never heard of Error Handling before or Try/Catch. After google searches and youtube videos, I came to the understanding that PowerShell can continue executing a script when it encounters certain non-terminating errors. Error Handling helps with you being in control of what you want the script to do once it comes across an error (Stop, Write an error, Prompt the administrator to continue with the script, etc).
  * My first Try/Catch was for name validation. Since the majority of the SamAccountNames included an underscore, I created a Try/Catch that would show an error if an underscore wasn't included with the SamAccountName due to human error:
<img width="750" height="512" alt="New User Script" src="https://github.com/user-attachments/assets/32d0bd85-d389-440d-8d14-23b2d0a44662" />
  * Followed by Validation:
<img width="685" height="262" alt="try_catch success and error" src="https://github.com/user-attachments/assets/e3537f04-3ad3-45e1-8330-8623c2bd076d" />

  * My next Try/Catch focused on handling a user being created that already existed. This included two catches: one specifically for an existing user, and a second general catch for other errors that could prevent the new user from being created. The second catch would provide details about the error.
<img width="749" height="358" alt="Existing User Try_Catch" src="https://github.com/user-attachments/assets/70d19586-747e-4042-be09-310d80988e7c" />
  * Followed by Validation:
<img width="756" height="255" alt="try_catch success and error existing user" src="https://github.com/user-attachments/assets/1eba7872-8283-4db0-b8d7-33b902d007ea" />

* My final steps was exporting a report for both Users and Computers
