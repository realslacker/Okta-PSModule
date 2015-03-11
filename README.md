# Okta-PSModule Documentation
======================

This is the starting point for documentation on my powershell module/wrapper for the Okta API.
This is not to be confused with or in competition with the official Okta [Powershell/CSharp module] (https://github.com/okta/oktasdk-csharp/tree/master/Okta.Core.Automation)

I have been building and adding to this for a few years, and I still need the functionality it provides on a near daily basis. I figured it was time to share.

Contents
--------

### Getting Started
#Installation:
1. Download the module (git clone or download the zip)
2. Place the module in your PSModulePath hint [Read more about PSModulePath Here] (https://msdn.microsoft.com/en-us/library/dd878324%28v=vs.85%29.aspx)
```powershell
Write-Host $env:PSModulePath
```
3. [Getting an API Token](http://developer.okta.com/docs/getting_started/getting_a_token.html) "You'll need an API token before you can do anything"
4. Create a file called Okta_org.ps1 and save it in the directory with the Okta.psd1 and Okta.psm1 files.
```powershell
<# Okta_org.ps1 #>
# Verbose will print various infomative bits of information
[Boolean]$oktaVerbose = $true
# define the default Okta Org you want to use, useful if you have more than one.
[String]$oktaDefOrg = "prod"

[Hashtable]$oktaOrgs = @{
                        prod1 = [Hashtable]@{
                                            baseUrl  = [String]"https://yourdomain.okta.com"
                                            secToken = [String]"yourApiToken"
                                            enablePagination = [boolean]$true
                                            pageSize = [int]500
                                           }
                        prod2 = [Hashtable]@{
                                            baseUrl  = [String]"https://yourOtherdomain.okta.com"
                                            secToken = [String]"yourOtherApiToken"
                                            enablePagination = [boolean]$true
                                            pageSize = [int]500
                                           }
                        prev = [HashTable]@{
                                            baseUrl  = [String]"https://yourDomain.oktapreview.com"
                                            secToken = [String]"yourPreviewApiToken"
                                            enablePagination = [boolean]$true
                                            pageSize = [int]500
                                           }
                        }
```
#To Use:
1. Launch powershell (or even better, the powershell ise.
2. Import the Okta Module
3. User
```powershell 
PS C:\> Import-Module Okta

PS C:\> oktaGetUserbyID -oOrg prod -uid mbegan@gmail.com
[ GET https://varian.okta.com/api/v1/users/mbegan@gmail.com ]


id              : 00u103j904jPJACDTXXV
status          : ACTIVE
created         : 2014-12-23T22:50:41.000Z
activated       : 2014-12-23T22:50:41.000Z
statusChanged   : 2014-12-23T22:50:41.000Z
lastLogin       : 2015-01-30T23:48:05.000Z
lastUpdated     : 2015-01-30T23:41:41.000Z
passwordChanged : 2015-01-30T23:41:41.000Z
profile         : @{email=mbegan@gmail.com; firstName=Matt; lastName=Egan; login=mbegan@gmail.com; mobilePhone=; secondEmail=}
credentials     : @{password=; recovery_question=; provider=}
_links          : @{resetPassword=; resetFactors=; expirePassword=; forgotPassword=; changeRecoveryQuestion=; deactivate=; changePassword=}
```
Objects returned are normal powershell objects, you can treat them as such.

An example of something I do often.

```powershell
PS C:\Users\megan> $oktauser = oktaGetUserbyID -oOrg prev -uid mbegan@gmail.com
[ GET https://varian.oktapreview.com/api/v1/users/mbegan@gmail.com ]

PS C:\Users\megan> $groups = oktaGetGroupsbyUserId -oOrg prev -uid $oktauser.id
[ GET https://varian.oktapreview.com/api/v1/users/00u3j3jj2cLstvJL70h7/groups ]

PS C:\Users\megan> foreach ($group in $groups) {write-host $group.profile.name $group.id}
Everyone 00g326179lGHZOYPWXCD
okta.throwaway 00g3hyrge0QfpnvM80h7

PS C:\Users\megan> oktaDeleteUserfromGroup -oOrg prev -uid $oktauser.id -gid $groups[1].id
[ DELETE https://varian.oktapreview.com/api/v1/groups/00g3hyrge0QfpnvM80h7/users/00u3j3jj2cLstvJL70h7 ]
```

Pretty simple example, i do much more.

It supports pagination, so grabbing ALL of your users or groups is not a problem.

I'll add more details on usage later, if you have a specific use case ask away i'll post an example.

### Available Commands
- oktaActivateUserbyId
- oktaAddUseridtoGroupid
- oktaAdminExpirePasswordbyID
- oktaAdminUpdatePasswordbyID
- oktaChangePasswordbyID
- oktaChangeProfilebyID
- oktaCheckCreds
- oktaConvertGroupbyId
- oktaDeactivateUserbyID
- oktaDeleteGroupbyId
- oktaDeleteUserfromGroup
- oktaDelUserFromAllGroups
- oktaDelUseridfromAppid
- oktaDelUseridfromGroupid
- oktaExternalIdtoGUID
- oktaForgotPasswordbyId
- oktaGetActiveApps
- oktaGetAppbyId
- oktaGetAppGroups
- oktaGetAppProfilebyUserId
- oktaGetAppsbyUserId
- oktaGetFactorbyUser
- oktaGetFactorsbyUser
- oktaGetGroupbyId
- oktaGetGroupMembersbyId
- oktaGetGroupsAll
- oktaGetGroupsbyquery
- oktaGetGroupsbyUserId
- oktaGetprofilebyId
- oktaGetUserbyID
- oktaGetUsersbyAppID
- oktaListDeprovisionedUsers
- oktaListUsersbyQuery
- oktaListUsersbyStatus
- oktaNewPassword
- oktaNewUser
- oktaProcessHeaderLink
- oktaPutProfileupdate
- oktaResetFactorbyUser
- oktaResetFactorsbyUser
- oktaResetPasswordbyID
- oktaSetAppidCredentialUsername
- oktaUnlockUserbyId
- oktaUpdateAppExternalIdbyUserId
- oktaUpdateAppProfilebyUserId
- oktaUpdateUserbyID
- oktaUpdateUserProfilebyID
- oktaVerifyMFAnswerbyUser
- oktaVerifyOTPbyUser