<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Configure Group Policy
- Dealing with account lockouts
- Enabling and Disabling Accounts

<h2>Deployment and Configuration Steps</h2>

<p>
First thing I need to do is logon to the Domain Contoller Virtual Machine so I can access the Group Policy Managment Console (GPMC). To do this we search gpmc.msc and hit enter and it should open right up. In the GPMC, I'm going to navigate to the mydomain.com domain I created and when I expand the list under it the first thing I am shown is a Default Domain Policy and I am going to edit this one to have the rules and policies I need to demonstrate this lab. After I have right clicked the Default Domain Policy I select edit and navigate to the Account Lockout Policy which is found in the Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.
<br />

![image](https://github.com/user-attachments/assets/32654a6c-deb0-4d83-9771-0f470be19f11)


<p>
After I have right clicked the Default Domain Policy I select edit and navigate to the Account Lockout Policy which is found in the Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy. Just for the convientent purpose of this lab I am going to configure the Account Lockout Policies to be pretty short. You can see what setting I set for each policy in the screenshot below. The red square is the first sentence in how I explained how to get to the Account Policies and the green square is the rules I set for each Policy.
</p>
<br />

![image](https://github.com/user-attachments/assets/c62367da-43f3-4924-975d-58417bb61d6a)

<p>
Now to update this setting to each Client PC (in my case for this lab its just one VM) you can wait for the Group Policy to propagate automatically, or you can force an update immediately. I am going to force the update immediatley by logging on to my Client-1 VM and running the command gpupdate /force in the administrative command line.
</p>
<br />

![image](https://github.com/user-attachments/assets/73b9fd61-8ba6-4e97-808c-c76b52e2e976)


<p>
To make sure that the update is applied and it is using the Default Domain Policy I configured I will run the command gpresult /r in the administrative command line and it will show me the Applied Group Policy Objects. 
</p>
<br />

![image](https://github.com/user-attachments/assets/5677b986-ea7a-4e4d-b549-4dcf4701ae12)

<p>
It is now time to logout of my Client-1 VM and log in as a random user and purposeley mess up the log in credentials 5 times and make sure the Account is locked out for 30 minutes. After the fifth failure I was given that little popup window mention the users account I was trying to log in as has been locked out.
</p>
<br />

![image](https://github.com/user-attachments/assets/81fe5953-739d-4c87-b314-335ca3c9e40e)

<p>
Now I will switch back to my DC-1 Domain Controller VM and check the users properties through the Active Directory Users and Computers Tool. To search through all the users in the _EMPLOYEES folder would take a long time as there is over 5000 so I can right click the domain mydomain.com and select find and search for the user that way. In this example I used the random user qiv.top and as you can see in the green box the account has been locked out and I can check the box to unlock the account. I am going to unlock the account and now log in as qiv.top with the correct credentials to see if it has worked.
</p>
<br />

![image](https://github.com/user-attachments/assets/e05140dc-dd80-4684-8bbd-1eb574b0b4f2)

<p>
It has worked and the account is unlocked!
</p>
<br />

![image](https://github.com/user-attachments/assets/a998ea0f-d39b-4952-8f8f-ccb08bbec659)

<p>
I am going to pivot now and demonstrate how to disable and re-enable the user qiv.top's account from the DC-1 Domain Controller PC. With the same steps from before about finding a user in a folder full of thousands of users I will use the Find method when you right click the domain in the Active Directory Users and Computers Tool. Once the user is found you can just simply right click the user and get a few valuable options like disable or reset password. I am going to click Disable Account. Now I will try to log in to the account on my Client-1 VM and observe what happens.
</p>
<br />

![image](https://github.com/user-attachments/assets/6c944133-431d-42e4-9502-0f8adede3ee0)
![image](https://github.com/user-attachments/assets/74d0ff52-cb8f-4909-9c9e-373486dd2814)


<p>
I am shown this pop up window when I attempt to log in as my disabled user account qiv.top
</p>
<br />

![image](https://github.com/user-attachments/assets/e5f7f8f6-415c-4add-b716-c87ba3d7f9b5)

<p>
If I Enable the account with the same way I disabled it then it will be all good and can log in again. 
</p>
<br />
