<p align="center"><img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/></p>

<h1>Group Policy</h1>
In this tutorial, we will use Group Policy Editor in Azure Virtual Machines to Configure Password Lockout
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Group Policy Editor

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2022

<h2>High-Level Steps</h2>

- Configure Account Lockout
- Update Group Policy
- Test Policy
<br />

<h2>Configuring Account Lockout Threshold Using Group Policy</h2>

Log in to the Domain Controller using an admin account
<p><img src="https://i.imgur.com/pB5GBpH.png" height="80%" width="80%"/></p>
<br />
Type "gpmc.msc" in the Windows search bar to open Group Policy Management Controller.
<p><img src="https://i.imgur.com/ka0fEDX.png" height="80%" width="80%"/></p>
<br />
Click the drop box next to "mydomain.com". Open "Group Policy Objects".
<p><img src="https://i.imgur.com/4r6ET9u.png" height="80%" width="80%"/></p>
<br />
Right-click "Default Domain Policy". Click "Edit".
<p><img src="https://i.imgur.com/HjFOpXJ.png" height="80%" width="80%"/></p>
<br />
Navigate to "Account Lockout Policy" by click the drop box next to Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Account Policies -> Account Lockout Policy
<p><img src="https://i.imgur.com/txGIY1i.png" height="80%" width="80%"/></p>
<br />
Double-click "Account Lockout Duration".
<p><img src="https://i.imgur.com/CmV2zdW.png" height="80%" width="80%"/></p>
<br />
Check the box next to "Define this policy". Set the lockout time to 30 minutes. Click "Apply". This setting determines how long an account is locked after the maximum amount of failed attempts has been reached.
<p><img src="https://i.imgur.com/mNbvPGY.png" height="80%" width="80%"/></p>
<br />
Click "OK" to allow suggested changes to be made to other settings.
<p><img src="https://i.imgur.com/SpiS6Oz.png" height="80%" width="80%"/></p>
<br />
Click "OK".
<p><img src="https://i.imgur.com/LEujpBi.png" height="80%" width="80%"/></p>
<br />
Account lockout threshold is the amount of attempts a user has before the account will become locked. This was already set to 5 attempts, which is fine for this lab.
<p><img src="https://i.imgur.com/W0vTcrn.png" height="80%" width="80%"/></p>
<br />
Double-click "Allow Administrator account locked". Mark the box to define the policy setting and mark "disabled", and click "Apply". We disabled admin account lockout for the purposes of this lab to ensure we do not lose access.
<p><img src="https://i.imgur.com/vo0JRWD.png" height="80%" width="80%"/></p>
<br />
Double-click "Reset account lockout counter afer", change the setting to 15 minutes. This setting changes how long the system will remember invalid password attempts.
<p><img src="https://i.imgur.com/WgfIHSO.png" height="80%" width="80%"/></p>
<br />
Exit out of Group Policy Editor. In Group Policy Management, right-click "_EMPLOYEES", and click "Link an existing GPO". Since we edited the default policy rather than create a new one, this is not necessary, but this will help us verify the policy change.
<p><img src="https://i.imgur.com/bAZFep0.png" height="80%" width="80%"/></p>
<br />
Select the GPO you would like to link top the OU.
<p><img src="https://i.imgur.com/v2Fhlhd.png" height="80%" width="80%"/></p>
<br />
Click "Close"
<p><img src="https://i.imgur.com/IFdGNh1.png" height="80%" width="80%"/></p>
<br />
Scroll down and observe the modified settings under "Account Policies/Account Lockout Policy". The settings should reflect the changes we just made.
<p><img src="https://i.imgur.com/JldurPm.png" height="80%" width="80%"/></p>
<br />

Account lockout threshold is now set up and linked to the OU.



<h2>Updating Group Policy</h2>

To update computers to the new Group Policy, we need to log in to the client computer as an admin.
<p><img src="https://i.imgur.com/X25T24E.png" height="80%" width="80%"/></p>
<br />
In the Windows search bar, type "CMD", run the command prompt as an administrator.
<p><img src="https://i.imgur.com/PEQG4lI.png" height="80%" width="80%"/></p>
<br />
Type "gpupdate /force" to update the group policy on the client computer. The prompt should say it has completed successfully.
<p><img src="https://i.imgur.com/EDsi4K6.png" height="80%" width="80%"/></p>
<br />
Type "gpresult /r".
<p><img src="https://i.imgur.com/KUnfXZJ.png" height="80%" width="80%"/></p>
<br />
Scroll down and observe under "Applied Group Policy Objects" the GPO we edited should be there. In this case it is "Default Domain Policy"
<p><img src="https://i.imgur.com/1oCx93y.png" height="80%" width="80%"/></p>
<br />
Press CTRL + "R". type "logoff".
<p><img src="https://i.imgur.com/1VdmxNK.png" height="80%" width="80%"/></p>
<br />
In the domain controller, select a user from the OU. in this case, "hiv.kug" will be used for testing purposes.
<p><img src="https://i.imgur.com/b063Cec.png" height="80%" width="80%"/></p>
<br />
Enter the domain user and login using an incorrect password multiple times until the account is locked.
<p><img src="https://i.imgur.com/q4nuAUk.png" height="80%" width="80%"/></p>
<br />
Remote Desktop will display an error once the account has been locked.
<p><img src="https://i.imgur.com/a5EAWqV.png" height="80%" width="80%"/></p>
<br />

The Group Policy is now updated!



<h2>Testing Group Policy</h2>

In the domain controller, open ADUC and select a user from the OU. in this case, "hiv.kug" will be used for testing purposes.
<p><img src="https://i.imgur.com/b063Cec.png" height="80%" width="80%"/></p>
<br />
Enter the domain user and login using an incorrect password multiple times until the account is locked.
<p><img src="https://i.imgur.com/q4nuAUk.png" height="80%" width="80%"/></p>
<br />
Remote Desktop will display an error once the account has been locked.
<p><img src="https://i.imgur.com/a5EAWqV.png" height="80%" width="80%"/></p>
<br />
In ADUC, right-click the domain, click "Find".
<p><img src="https://i.imgur.com/TtQ7pLt.png" height="80%" width="80%"/></p>
<br />
Type name of the user experiencing lockout. Click "Find Now". Double-click the user.
<p><img src="https://i.imgur.com/jm1wO8d.png" height="80%" width="80%"/></p>
<br />
Click the "Account" tab
<p><img src="https://i.imgur.com/XtgCbTq.png" height="80%" width="80%"/></p>
<br />
Check the box labeled "Unlock Account". Click "Apply".
<p><img src="https://i.imgur.com/ynB009M.png" height="80%" width="80%"/></p>
<br />

The account is now unlocked and the user may log in!
