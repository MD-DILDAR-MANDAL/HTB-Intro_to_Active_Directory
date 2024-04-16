# Let us consider the following case:

![htb-iamge](https://academy.hackthebox.com/storage/modules/74/helping-out.png)

# Task 1: Manage Users

Our first task of the day includes adding a few new-hire users into AD. We are just going to create them under the "inlanefreight.local" scope, drilling down into the "Corp > Employees > HQ-NYC > IT " folder structure for now. Once we create our other groups, we will move them into the new folders. You can utilize the Active Directory PowerShell module (New-ADUser), the Active Directory Users and Computers snap-in, or MMC to perform these actions.

Users to Add:

- Andromeda Cepheus
- Orion Starchaser
- Artemis Callisto

Each user should have the following attributes set, along with their name:
#### Attribute
- full name
- email (first-initial.lastname@inlanefreight.local) ( - ex. j.smith@inlanefreight.local )
- display name
- User must change password at next logon

Once we have added our new hires, take a quick second and remove a few old user accounts found in an audit that are no longer required.
#### Users to Remove
- Mike O'Hare
- Paul Valencia

Lastly Adam Masters has submitted a trouble ticket over the phone saying his account is locked because he typed his password wrong too many times. The helpdesk has verified his identity and that his Cyber awareness training is up to date. The ticket requests that you unlock his user account and force him to change his password at the next login.

## Add new users :

```
New-ADUser -Name "Andromeda Cepheus" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="a.cepheus@inlanefreight.local"} -DisplayName “Andromeda”
```
```
New-ADUser -Name "Orion Starchaser" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="o.starchaser@inlanefreight.local"} -DisplayName “Orion”
```
```
New-ADUser -Name "Artemis Callisto" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="a.callisto@inlanefreight.local"} -DisplayName “Artemis”
```
## Delete AD User :
```
Remove-ADUser -Identity Mike O'Hare
```
```
Remove-ADUser -Identity Paul Valencia
```

## Unlock User : 
```
Unlock-ADAccount -Identity amasters
```
## Reset User : 

```
Set-ADAccountPassword -Identity 'amasters' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ssw0rdReset!" -Force)
```
```
Set-ADUser -Identity amasters -ChangePasswordAtLogon $trues
```
# Task 2: Manage Groups and Other Organizational Units

Next up for us is to create a new Security Group called Analysts and then add our new hires into the group. This group should also be nested in an OU named the same under the IT hive. The New-ADOrganizationalUnit PowerShell command should enable you to quickly add a new security group. We can also utilize the AD Users and Computers snap-in like in Task-1 to complete this task.

## Nested OU create : 

```
New-ADOrganizationalUnit -Name “Security Analysts” -Path “OU=IT,OU=HQ-NYC,OU=Employees,OU=CORP,DC=INLANEFREIGHT,DC=LOCAL”
```
## New Group in OU : 

```
New-ADGroup -Name "Security Analysts" -SamAccountName analysts -GroupCategory Security -GroupScope Global -DisplayName "Security Analysts" -Path "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" -Description "Members of this group are Security Analysts under the IT OU"
```

## Add User To Group : 

```
Add-ADGroupMember -Identity analysts -Members Andromeda,Orion,Artemis
```

# Task 3: Manage Group Policy Objects
Next, we have been asked to duplicate the group policy Logon Banner, rename it Security Analysts Control, and modify it to work for the new Analysts OU. We will need to make the following changes to the Policy Object:

- we will be modifying the Password policy settings for users in this group and expressly allowing users to access PowerShell and CMD since their daily duties require it.
- For computer settings, we need to ensure the Logon Banner is applied and that removable media is blocked from access.
- Once done, make sure the Group Policy is applied to the Security Analysts OU. 

## Duplicating Group Policy Object : 

```
Copy-GPO -SourceName "Logon Banner" -TargetName "Security Analysts Control"
```

## Link The Group Policy Object To OU : 

```
New-GPLink -Name "Security Analysts Control" -Target "ou=Security Analysts,ou=IT,OU=HQ-NYC,OU=Employees,OU=Corp,dc=INLANEFREIGHT,dc=LOCAL" -LinkEnabled Yes
```
## Modify GPO use GPMC :

To modify our new policy object:
- We need to open GPMC and expand the Group Policy Objects hive so we can see what GPOs exist.

- Right-click on the policy object we wish to modify and select "Edit.  The Group Policy Management Editor should pop up in a new window.

- From here, we have several options to enable or disable.

- We need to modify the removable media settings and ensure they are  set to block any removable media from access. We will expressly allow  security analysts to access PowerShell and CMD since their daily duties  require it.

    - location of removable media policy settings = User Configuration > Policies > Administrative Templates > System > Removable Storage Access.

    - Location of Command Prompt settings = User Configuration > Policies > Administrative Templates > System.


- For Computer settings, we need to ensure the Logon Banner is applied and that the password policy settings for this group are strengthened.

    - Location of Logon Banner settings = Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options.

    - For reference, this setting should already be enabled since the GPO we copied was for a Logon Banner. We are validating the settings and ensuring it is enabled and applied.

    - Location of Password Policy settings = Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy.

## User Configuration Group Policies : 

We will be modifying the policies affecting users access to the command prompt as well as their ability to use removeable media.

![](https://academy.hackthebox.com/storage/modules/74/edit-policy.png)
![](https://academy.hackthebox.com/storage/modules/74/storage-1.png)
![](https://academy.hackthebox.com/storage/modules/74/storage-2.png)
![](https://academy.hackthebox.com/storage/modules/74/storage-3.png)
![](https://academy.hackthebox.com/storage/modules/74/storage-4.png)
![](https://academy.hackthebox.com/storage/modules/74/cmd-1.png)
![](https://academy.hackthebox.com/storage/modules/74/cmd-2.png)
![](https://academy.hackthebox.com/storage/modules/74/cmd-3.png)
![](https://academy.hackthebox.com/storage/modules/74/cmd-4.png)

## Computer Configuration Group Policies : 

Modifying group policies that affect computers in the group. We will be modifying the policies affecting the Logon Banner for the host, and setting a more restrictive password policy.

![](https://academy.hackthebox.com/storage/modules/74/banner-1.png)
![](https://academy.hackthebox.com/storage/modules/74/banner-2.png)
![](https://academy.hackthebox.com/storage/modules/74/banner-3.png)
![](https://academy.hackthebox.com/storage/modules/74/password-1.png)
![](https://academy.hackthebox.com/storage/modules/74/password-2.png)
![](https://academy.hackthebox.com/storage/modules/74/password-3.png)
![](https://academy.hackthebox.com/storage/modules/74/password-4.png)
![](https://academy.hackthebox.com/storage/modules/74/password-5.png)
![](https://academy.hackthebox.com/storage/modules/74/password-6.png)
