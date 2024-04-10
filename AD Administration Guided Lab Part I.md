# Task 1: Manage Users

## Add new users

```
New-ADUser -Name "Andromeda Cepheus" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="a.cepheus@inlanefreight.local"} -DisplayName “Andromeda”
```
```
New-ADUser -Name "Orion Starchaser" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="o.starchaser@inlanefreight.local"} -DisplayName “Orion”
```
```
New-ADUser -Name "Artemis Callisto" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="a.callisto@inlanefreight.local"} -DisplayName “Artemis”
```
## Deleted AD User
```
Remove-ADUser -Identity Mike O'Hare
```
```
Remove-ADUser -Identity Paul Valencia
```