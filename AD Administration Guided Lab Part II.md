# Task 4: Add and Remove Computers To The Domain

Our new users will need computers to perform their daily duties. The helpdesk has just finished provisioning them and requires us to add them to the INLANEFREIGHT domain. Since these analyst positions are new, we will need to ensure that the hosts end up in the correct OU once they join the domain so that group policy can take effect properly.

The host we need to join to the INLANEFREIGHT domain is named: ACADEMY-IAD-W10 and has the following credentials for use to login and finish the provisioning process:

- User == image
- Password == Academy_student_AD!

Once you have access to the host, utilize your ```htb-student_adm: Academy_student_DA!``` account to join the host to the domain.

## Host Join : 

```
Add-Computer -DomainName INLANEFREIGHT.LOCAL -Credential INLANEFREIGHT\HTB-student_adm -Restart
```
## Add Remote Computer to Domain

```
Add-Computer -ComputerName ACADEMY-IAD-W10 -LocalCredential ACADEMY-IAD-W10\image -DomainName INLANEFREIGHT.LOCAL -Credential INLANEFREIGHT\htb-student_adm -Restart
```
When we added the computer to the domain, we did not stage an AD object for it in the OU we wanted the computer in beforehand, so we have to move it to the correct OU now. 

### Lets Check OU Membership of a Host

```
Get-ADComputer -Identity "ACADEMY-IAD-W10" -Properties * | select CN,CanonicalName,IPv4Address
```
The CanonicalName property (seen above) will tell us the full path of the host by printing out the name in the format "Domain/OU/Name." We can use this to locate the host and validate where it is in our AD structure.

Utilizing the ADUC snap-in, we can also move computer objects pretty quickly :

## Move A Computer Object To A New OU : 

![](https://academy.hackthebox.com/storage/modules/74/moveou1.png)
![](https://academy.hackthebox.com/storage/modules/74/moveou2.png)
![](https://academy.hackthebox.com/storage/modules/74/moveou3.png)
![](https://academy.hackthebox.com/storage/modules/74/moveou4.png)