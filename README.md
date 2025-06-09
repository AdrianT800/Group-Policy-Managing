![1035255](https://github.com/user-attachments/assets/7cdae74a-7836-4365-af4e-737124531edf)

# Group Policy and Managing
This guide outlines how to manage account lockouts by configuring Group Policy settings, handle password resets, and enable or disable user accounts. Additionally, it covers how to monitor authentication and security logs to observe login activity and potential security issues.<br />


<h2>Video Demonstration</h2>

-   ### [Google Drive: Group Policy and Managing](https://drive.google.com/file/d/1NqbU-I-uYD5NGY7tGpdfJ1VvP6TgEaMc/view?usp=drive_link)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Resources Group, Virtual Machines/Computer, Virtual Machines/Window Server, Virtual Network and Subnet)
- Remote Desktop
- Powershell
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows 10 (Pro, version 22H2 - x64 Gen2)
- Windows Server 2022 Datacenter: (Azure Edition - x64 Gen2)

<h2>Group Policy and Managing step-by-step</h2>

- Step 1: Dealing with Account Lockouts
- Step 2: Configure Group Policy to Lockout the account after 5 attempts
- Step 3: Resetting Passwords
- Step 4: Enabling and Disabling Accounts
- Step 5: Observing Logs

<h2>Lifecycle Stages</h2>

Step 1: Dealing with Account Lockouts
<p>
</p>
<p>
To begin addressing account lockouts, log into DC-1, open Active Directory Users and Computers (ADUC), select a user account, and attempt to log into Client-1 using an incorrect password 10 times. You’ll notice that the account does not lock out — this is because Group Policy has not yet been configured.
</p>
<br />

Step 2: Configure Group Policy to Lockout the account after 5 attempts
<p>
<img src="https://i.imgur.com/vuohGM7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On DC-1, open the Group Policy Management Console (GPMC) by clicking Start, typing gpmc.msc, and pressing Enter. In GPMC, navigate to Forest: mydomain.com > Domains > mydomain.com > Group Policy Objects, right- click Default Domain Policy, and select Edit to open the Group Policy Management Editor.
<br />

<p>
<img src="https://i.imgur.com/oHaOFIM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From there, expand: Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy. Configure the settings by double-clicking Account Lockout Duration, enabling the policy, setting it to 30 minutes, and clicking Apply and OK.
</p>
<br />

<p>
<img src="https://i.imgur.com/GUJsnBM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Then, set the Account Lockout Threshold to 5 attempts, and apply the changes.
</p>
<br />

<p>
<img src="https://i.imgur.com/rceyQTu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Return to Group Policy Management GPM, select Default Domain Policy, and review the updated settings. To apply the policy, you can either wait for the default 90-minute propagation period or force an update.
</p>
<br />

<p>
<img src="https://i.imgur.com/DV4z7Uf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To do the latter, log into Client-1 as jane_admin (mydomain.com\jane_admin, password: Vmdemo12345$), open Command Prompt as an administrator, and run gpupdate /force. After the update, run gpresult /r to confirm the policy has applied. Log out of Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/gmPTGbA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To verify the policy, attempt to log into Client-1 using the selected account and enter an incorrect password five times. The account should now be locked. Even when using the correct password, the account remains locked.
</p>
<br />

<p>
<img src="https://i.imgur.com/SuGAzkG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To unlock it, return to DC-1, open ADUC, right-click mydomain, select Find, search for the account, open its properties, go to the Account tab, check Unlock Account, and apply the changes. You should now be able to log into Client-1 successfully.
</p>
<br />

Step 3: Resetting Passwords
<p>
<img src="https://i.imgur.com/VnwRGOr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To reset a password, go to ADUC on DC-1, search for the account, right-click it, select Reset Password, and set a new password (e.g., Password2). Test the new password by logging in, then log out.
</p>
<br />

Step 4: Enabling and Disabling Accounts
<p>
<img src="https://i.imgur.com/Inv8xUD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To disable and re-enable an account, go to ADUC in DC-1, search for the account, right-click it, and select Disable Account.
</p>
<br />

<p>
<img src="https://i.imgur.com/fDoyffr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Attempt to log into Client-1 and observe the error message. Then, return to ADUC, re-enable the account, and try logging in again.
</p>
<br />

Step 5: Observing Logs
<p>
<img src="https://i.imgur.com/RaF0nv1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, to observe security logs, log into Client-1 with your account, open the Event Viewer by typing eventvwr.msc in the search bar, and run it as jane_admin. Expand Windows Logs > Security, use the Find feature to search for the account, and review the logs for Audit Failures, particularly those related to login attempts.
</p>
<br />
