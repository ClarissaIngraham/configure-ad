<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1> Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

First we will create two VM's(Virtal Machines) with the same virtal network. One VM is function as a Domain Controller(DC-1) and the other VM will funtion as a Client machine (Client-1). We will assign a static IP address to the Domain Controller, as it will provide Active Directory services to the Client. The Client machine will be joined to the domain, and we will configure its DNS settings to use the Domain Controller as its DNS server.

A static IP address is an IP address that doesn't change. We are going to be using this because it is going to provide a consistent address for communication over the network for DC-1 because we are going to be using DC-1 as the server and as the DNS server for Client-1.
<p>
  
![image](https://github.com/user-attachments/assets/a747979e-83b9-4721-830d-8d64e09157a7)

</p>
<p>

![image](https://github.com/user-attachments/assets/c129f076-b3bd-426a-aad4-f9a4df701a52)

Client-1 will establish connectivity with DC-1, and to confirm this connection, a ping test will be conducted from Client-1 to DC-1. In order for this to work, we will first login to DC-1 and turn of the domain,public, and private firewalls. Once that is applied, we are going to open up powershell in VM-Client-1 and we should be able to successfully ping DC-1, confirming connectivity. (Enter ping 10.0.0.4 in powershell)
</p>

<p>
  
![image](https://github.com/user-attachments/assets/b70012a5-f04b-446c-9191-1b62859485ab)

![image](https://github.com/user-attachments/assets/c69fc5a1-8772-4694-81d0-77d30a526d24)

</p>
<p>
We will not go back to our DC-1 VM and install Active Directory Domain Services. To proceed with this process we will click on the start icon > Server Manager > Add roles and features. When we get to the session of the installation where it says server roles, we will check off "Active Directory Domain Services" and then click add features then continue with the installation. 

Next, promote the VM to a Domain Controller, set up a new forest with the domain name "mydomain.com," and then restart the system. After restarting, log back into DC-1 as the user "mydomain.com\labuser."

Your screen should you look like the one below if AD is installed on your device.
</p>

<p>
  
![image](https://github.com/user-attachments/assets/7b9ed7b5-0d1f-4926-84cd-0e2966a05b9a)

</p>
<p>
So now that we have installed Active Directory(AD), we will create Organizational Units (OU) with AD. We're going create two folders, one for employees called “_EMPLOYEES” and another for admins called “_ADMINS”.

To do this, right-click on the domain area, select New > Organizational Unit, and enter the name and click okay. Next, open your newly created OU > right-click > select New > User > enter Jane Doe > assign her the role of Admin with the username Jane_admin. Finally, we will add Jane to the Domain Admin to grant her the appropriate permissions. We can now use Jane_admin as the administrator account.

![image](https://github.com/user-attachments/assets/7c8420c9-35b5-4d9e-bfcd-b2adab34b117)
</p>

Next we will join Client-1 to the domain. To do this navigate to the system settings and select about. On the right, choose rename this PC (advanced), then select change under the domain settings. Enter "mydomain.com" as the domain name, followed by the credentials for "mydomain.com\labuser." After completing these steps, the system will restart, and Client-1 will be successfully joined to "mydomain.com."

![image](https://github.com/user-attachments/assets/119410dd-38b6-46ee-8535-417d69b1329a)

Now Client-1 is successfully joined to the domain. Next, we’ll configure Remote Desktop access for non-administrative users on Client-1. Start by logging into Client-1 as an administrator right click on the windows button and click on system. While on that screen in the about section click on remote desktop > select users that can remotely access this PC and grant "domain users" access to Remote Desktop. 

When these steps are submitted correctly, users will be able to log into Client-1 remotely.

![image](https://github.com/user-attachments/assets/0c745116-e183-4867-8864-cc2d4e514442)

Last and not least we will now confirm that standard users can successfully access Client-1 via Remote Desktop. We will open up Powershell in Client-1 and we’ll use a PowerShell script to generate a large number of users in the domain. Once we have created the users, we will use one of the user's credentials to verify that we could log into Client-1 using the user's credentials.

![image](https://github.com/user-attachments/assets/1722679e-20e2-4d4a-affc-f70f298ac7e2)

![image](https://github.com/user-attachments/assets/39319d2f-c822-47e4-b46d-b6012f897260)
