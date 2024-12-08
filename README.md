<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Configuring a domain on the domain controller VM
- Creating a domain admin
- Join the Client VM to the domain
- Create a normal client user 


<h2>Configuring Domain</h2>

![image](https://github.com/user-attachments/assets/d2956577-6682-466e-90ae-3db84d73d98c)

<p>
To start, remote into both servers we created from the last portion of the walkthrough. We will first install active directory domain services on the domain controller VM. To do this, from the start menu, find the server manager > Add roles and features. Keep clicking next until the server roles menu appears. Check the box that says "Active Directory Domain Services" then Add Features. Then go through the rest of the installation leaving everything as default and click Install.
</p>
<br />

![image](https://github.com/user-attachments/assets/d2bf81d3-27d5-4909-a1b8-986497949847)

<p>
Next, click on the flag in the top right and then click "Promote this server to a domain controller" > Add a new forest > Root domain name = mydomain.com > Next > Choose a password for the DSRM (this is not going to be used but is required for setting up AD. Then click Next and uncheck Create DNS delegation. Leave everything else as default values and click Install at the end. After the installation completes, the server should automatically restart itself. 
</p>
<br />

![image](https://github.com/user-attachments/assets/d800875e-9d6d-434c-aedd-0c57186c9039)


<p>
After the restart, get back into the server with remote desktop. Doing this will require us to enter the domain name\user in the username feild as the domain controller now requires a domain source. 
</p>
<br />

<h2>Creating A Domain Admin</h2>

![image](https://github.com/user-attachments/assets/bc252d6d-dcbb-4ffc-b063-2dab32e72f51)

<p>
When you are logged into the VM again, click on the start menu > Windows Administrative Tools > Active Directory Users and Computers. We are going to be creating a domain admin. Before we can do this, lets create two Organizational Units (OU's) called _EMPLOYEES and _ADMINS . Right click on mydomain.com > New > Organizational Unit. 
</p>
<br />

![image](https://github.com/user-attachments/assets/07e2e20f-3c04-4c6b-aa1f-de241a3883d7)

<p>
Now, we are going to create the admin user. From within the ADMIN OU, right click > New > User. Fill out the information with whatever you want, but uncheck the "User must change password at next login" box. Also make sure to take note of the credentials you use because we will be using this user from now on to access the VM.
</p>
<br />

![image](https://github.com/user-attachments/assets/732e513b-460a-4f39-9645-47c2d145ac32)

<p>
Next, to make this user an admin, we need to add them to a security group. To do so, right click the user > Member Of > Add. Type "domain admins" in the box and click Chekc Names > Ok > Apply > Ok.
</p>
<br />

<h2>Adding Client to the Domain</h2>

![image](https://github.com/user-attachments/assets/51472794-8aec-4fe8-a39f-97ddece15661)

<p>
Now, log out of the machine and reconnect using the credentials of the user we just created. Also, use remote desktop to access the client VM as we are going to join it to the domain we configured. To do this, from within the client VM, right click start > System > Rename this PC (advanced) > Computer Name > Change. Switch Member of to Domain, and enter the domain name in the box (mydomain.com) then click ok. It should ask for credentials of an admin to make this change. We can use the admin credentials for the admin user we created before. This should result in a message that says welcome to the domain and then ask you to restart your sever. 
</p>
<br />

![image](https://github.com/user-attachments/assets/a9a8dfe8-8724-4ad5-9b87-44640f089153)

![image](https://github.com/user-attachments/assets/1d3a223b-8c3e-44d1-9a8b-2d4664b5550c)


<p>
To confirm that the client server is on the domain, we can open the Active Directory Users and Computers > mydomain.com > Computers. The client server should now appear in this list. Now, let's create a new OU called _CLIENTS and drag the client computer in Computers into the _CLIENTS OU. 
</p>
<br />

![image](https://github.com/user-attachments/assets/69677af1-cc7a-434f-8da2-a9f75331b256)


<p>
Next, login to the client server using the admin user credentials. We are able to do this because the client server was added to the domain. We want to configure the CLient VM so that any domain user can use their credentials to access the VM. Right click start > System > remote desktop > Select users that can remotely access this PC > Add > Type "domain users" in the box and click Check Names > Ok > Ok. 
</p>
<br />

<h2>Creaating A Non-Admin Employee</h2>

<p>
From within the domain controller, let's now create an employee in the Active Directory Users and Computers window that won't be admins. Click on the _EMPLOYEES OU > Right click > New > User. Use whatever credentials you want. Now try to login to the client VM using the credentials of the employee you just made. 
</p>
<br />

![image](https://github.com/user-attachments/assets/dd726342-2d8e-4d02-810e-d3f67015cb62)


<p>
Good Job! After logging into the client VM with the new employee account, we can move on to the next part of this Active Directory walkthrough.
</p>
<br />



