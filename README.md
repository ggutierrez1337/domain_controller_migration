# domain_controller_migration

# Installing Windows Server 2019 (A19)

1. Install Windows Server 2019 iso file from the Microsoft Center
2. In VirtualBox, Create a New VM with at least 4GB of RAM and 50GB of Storage

![Screenshot_(365)](https://github.com/user-attachments/assets/bb36330b-ed3f-4bb7-b55b-fceadfb2c224)

1. Right click the VM and select **Settings.** Under Settings, navigate to **Storage** and select the **Empty** Drive. For **Optical Drive,** Select the **Disk** icon and click **Choose a disk file…**
   
![image 1](https://github.com/user-attachments/assets/fecadf36-4258-4d06-b8e6-13c041f6de4c)

![image 2](https://github.com/user-attachments/assets/d17661c1-88a4-41f3-9c99-1b7e85d5b5c4)

![image 3](https://github.com/user-attachments/assets/f0232437-06c2-4b43-8a30-0d215427b394)


 b.  Windows Server 2019 will now be added to the VM


 c.  Next, under the VM Settings, go to **Network.** Ensure the VM is **Attached to: Internal Network** for the desired adapter in VirtualBox

![image 4](https://github.com/user-attachments/assets/342cd0a2-4dad-4bc8-bc6e-89a396059e80)

1. Once Settings are configured, Start the Virtual Machine. Will be brought to the Windows Setup screen in order to do a clean install of Windows Server 2019

![image 5](https://github.com/user-attachments/assets/eac426c4-c1bb-4c14-ba96-833c62caaa78)

1. For **Operating System,** select **Windows Server 2019 Datacenter Evaluation (Desktop Experience)**

![image 6](https://github.com/user-attachments/assets/12f3af90-9f76-4d6a-b9e2-e20c6bee7740)

 b.  In order to do a **clean install,** select **Custom: Install Windows only (advanced)** because an upgrade is not being done

![image 7](https://github.com/user-attachments/assets/046537f5-bafc-4133-a112-8cb707d9e50f)

 c.  Select the Virtual Hard Drive that was created along with the VM and choose **Next**

 d.  Installation will now begin

![image 8](https://github.com/user-attachments/assets/de0e403d-b334-49b9-9070-0ee743345185)

1. Once the VM has downloaded and installed Windows Server 2019 it reboot and prompt to enter password for the admin account on the machine

![image 9](https://github.com/user-attachments/assets/7df6fc41-36d7-4db9-ad58-6726359d1a2a)
![image 10](https://github.com/user-attachments/assets/929695e1-a9fd-447a-a299-a90b6b522964)


# Creating a a Forest Root Domain Controller

1. In Server Manager of A19, select **Local Server** and rename the server by selecting the **Computer Name → Change**

![image 11](https://github.com/user-attachments/assets/d0f169c0-2ace-4941-8652-45c66f8da217)
![image 12](https://github.com/user-attachments/assets/4efb1ad0-7e5f-4ebd-8abc-6cf0cf67be0a)


2. Again, in Local Server, next to **Ethernet** select **IPv4 address assigned by DHCP, IPv6 enabled**. Right click **Ethernet → Properties →** Disable **Internet Protocol Version 6 →** Select **Internet Protocol Version 4**

![image 13](https://github.com/user-attachments/assets/8f867200-e99b-4cbc-93e6-1825a31ec829)
![image 14](https://github.com/user-attachments/assets/6bafb14f-80d9-48f9-a758-ec2823f518e1)
![image 15](https://github.com/user-attachments/assets/70136fbd-50e4-465b-bbe7-18a2ca82f92d)



3. Set a static IP Address for the Server and ensure preferred DNS Server is a loopback address or the static IP assigned to the server. Once finished, **reboot** the server for settings to take affect

![image 16](https://github.com/user-attachments/assets/2d46f2b3-2c29-40a1-a31b-77f73b9189c6)

Now there is a server properly named along with correct IP Configurations added to it. Now Active Directory Domain Services needs to be installed

4. In Server Manager, select **Add role and features** and select Next until reaching the **Server Roles** tab. Check the box for **Active Directory Domain Services** (Will also install DNS as well). Select Next for the following tabs and then click **Install**

![image 17](https://github.com/user-attachments/assets/d9860b2c-087f-4692-b8a0-6d28077b9754)
![image 18](https://github.com/user-attachments/assets/9ea90e06-f911-4278-9538-a47037b1abd8)
![image 19](https://github.com/user-attachments/assets/9241b353-f7ea-40c3-9c79-f90c0c855a14)



5. Once installed, select the **flag icon** in Server Manager and click **Promote this server to a domain controller.** Since a brand new forest is being made, under **Select a deployment operation,** choose **Add a new forest** and name it

![image 20](https://github.com/user-attachments/assets/e1ed4edf-79fc-4e46-88f6-64409fbe7cd6)

6. **Domain Controller Options -** DNS will be installed and this server will be made a Global Catalog. Input a password for **Directory Service Restore Mode (DSRM)** (Would start the DC but not the AD database, therefore wont be able to login with AD credentials, but with this password created).  Continue selecting next. 

![image 21](https://github.com/user-attachments/assets/81d0e07f-2d41-4422-af28-932f2d25f061)

  b.  **Additional Options -** Will provide a **NetBios domain name.**

![image 22](https://github.com/user-attachments/assets/bd6461d0-4363-45df-93fa-b8c39f072d93)

 c.  **Paths -** Paths for AD DB, logfiles, and Sysvol. Leave as is unless one wants to change the drive for the paths (ie. C, D, E drives)

![image 23](https://github.com/user-attachments/assets/debe59ab-8a7a-469a-8073-dff48033a9b1)

 d. **Review Option -** Will be provided a summary of the Domain Controller configurations. All prerequisites will then pass and select **Install**

![image 24](https://github.com/user-attachments/assets/32fb4b0c-6042-4e06-a631-6931c9d3c50f)
![image 25](https://github.com/user-attachments/assets/f9fa3a08-4821-4af7-935f-e02e80b5ea39)


Once Server has rebooted, it will now be a fully functional Domain Controller.

7. In Server Manager Tools, go to **Active Directory Users and Computers**. Right click the domain and create a **New Organizational Unit** to simulate having some objects in the domain and within that OU create another OU. Within the nest OU, create a **New Group** and **User**

![image 26](https://github.com/user-attachments/assets/9713581c-19b7-4ae9-9466-e714722a56c5)
![image 27](https://github.com/user-attachments/assets/0c3c6af1-8e67-403a-9b9a-f56d4de56c02)
![image 28](https://github.com/user-attachments/assets/37bb0123-bc18-4f87-bb95-ab27fef5e789)
![image 29](https://github.com/user-attachments/assets/5c508a92-a4d0-45b6-b467-f92b798295cd)




8. Back in Server Manager Tools, go to **Group Policy Management.** Follow the path: **Expand Forest → Domains → WINSERV.local →** Right Click **Sales** OU and create a **GPO** for the OU

![image 30](https://github.com/user-attachments/assets/69041dd3-1d14-492e-8971-a35aa4ab661a)
![image 31](https://github.com/user-attachments/assets/3a8eb39e-03fb-4634-9a7e-e907d7f24416)


9. Under Server Manager Tools, select **DNS** and follow the path **Server_Name → Forward Lookup Zones → [Domain.Name](http://Domain.Name) →** and can see the name of the Server meaning everything is fine with this domain

![image 32](https://github.com/user-attachments/assets/c87e2695-0055-4dd9-8812-4e56d3dab88a)

All steps were conducted to deploy and provision Server A19 (Windows Server 2019).

# Install Windows Server 2019 on an Additional Server (B19)

Create another DC that runs Windows Server 2019 because every domain will have a minimum of 2 servers running in each environment for redundancy in the case that one fails user can still authenticate, DNS is still running, etc

1. Follow the same steps referring to **Installing Windows 2019** section as before. Only thing that will be changed for this VM is the **Name (B19 (2019 DC)).** There should now be **two VMs** for Windows Server 2019. Configure B19 as done with A19

![image 33](https://github.com/user-attachments/assets/c1f19235-ada9-47a8-afe6-409c1ec9909d)

Currently this server is not a member of the domain. Just a workgroup server in the environment (internal network)

# Confirm Connection Between Server A19 and B19

1. In both Server A19 and B19, navigate to **Windows Defender Firewall with Advanced Security.** Under **File and Printer Sharing (Echo Request-IMCPv4-In)** and **File and Printer Sharing (Echo Request-IMCPv6-In),** select **Enable** under Actions tab on the right hand side.
    1. This will allow for ping requests to be made between Windows machines to ensure that a host/client is active on a network

![image 34](https://github.com/user-attachments/assets/eb51fdab-c010-4716-bc80-2847dcdbf3c5)

2. Once done, in the **cmd** ping Server A19 from Server B19 and vice versa

![image 35](https://github.com/user-attachments/assets/a9caac8f-7af0-42be-b9df-79ffc323d51e)
![Screenshot_(414)](https://github.com/user-attachments/assets/4ed9f5f1-6e91-491b-8dfc-a3179db78a44)


# Installing AD and Adding Server B19 to Current Domain

1. In Server Manager for B19, select **Add Features** and follow the same steps when installing **Active Directory Domain Services** as done on Server A19
2. Once installed, select the **flag icon** in Server Manager and click **Promote this server to a domain controller.** Since this a new domain being added to an existing domain for replication, select the option **Add a domain controller to an existing domain**
    1. Specify the domain of Server A19 along with a user who has admin credentials for that server

![Screenshot_(407)](https://github.com/user-attachments/assets/b27d331a-0098-41e8-ac48-1fc96e286a4b)

3. Enter a new DSMR password for this server

![image 36](https://github.com/user-attachments/assets/78015ea1-0d12-4cb0-b30b-52adeafcc25a)

4. Under **Replicate from:** specify the server that B19 will replicate all information from

![image 37](https://github.com/user-attachments/assets/d5ffa791-4bb9-42bc-8345-52a5f51e537e)

5. Select next for all defaults and click **Install** to install AD Domain Services

![image 38](https://github.com/user-attachments/assets/439630b7-b9d0-4c0b-9b44-79e451acf685)

![image 39](https://github.com/user-attachments/assets/f6c76413-b01d-44a5-88c2-823e829d4baf)

6. Once done, server B19 will reboot. 

The following concludes the promotion and provision of Active Directory on Server B19 only

## Examining the B19 Server Environment

1. Navigate to **Active Directory Users and Computers** and check the **Domain Controller** OU. It can be seen that both Servers A19 and B19 are in the DC OU

![image 40](https://github.com/user-attachments/assets/2dbe2eac-6585-423c-b37f-c2903a53aa7c)

2. It can also be seen that the Sales OU, Sales Users OU, Sales Manager Group, and User were replicated to Server B19 as well

![image 41](https://github.com/user-attachments/assets/866653c9-20af-4135-8e76-e97c1459574c)

3. Go to **Group Policy Management.** It can be seen that the GPO set earlier on Server A19 has also been replicated to server B19

![Screenshot_(418)](https://github.com/user-attachments/assets/e45e10d6-8aa3-4d62-b73c-fbf8b8053b7f)

4. Select **WINSERV.local** and under the **Status** tab, select **Detect Now**. Under **Status details,** it specifies the information of bother Servers and can be seen that Server B19 is in sync with Server A19 meaning everything is replicating fine

![image 42](https://github.com/user-attachments/assets/f011cfe7-2f03-4e6d-99cc-8f6f7d50f799)

5. Go back to **Active Directory Users and Computers**. In the **Sales Users** OU, create a group called **Sales Executives.**

![image 43](https://github.com/user-attachments/assets/7499fd16-b1c8-4246-9669-9a185e4297fd)

6. Right click the Domain Controller and select **Change Domain Controller**. This changes the view allowing the user to **remotely connect to Server A19**

![image 44](https://github.com/user-attachments/assets/e460af65-1049-421e-95dd-e1e7fd3c5028)

![image 45](https://github.com/user-attachments/assets/461a782e-70a1-440f-a2ed-7dac9156ccde)

7. The **view** has now been changed to **A19**. It can be seen that in the Sales Users OU, the new group has been replicated over. By doing this, it eliminates the user from having to **switch from one DC to another**

![image 46](https://github.com/user-attachments/assets/a2ad5267-b24b-4d26-b6ca-17893f8d89d9)

8. Change back to B19 Domain Controller. Go to **Event Viewer** in the Server Manager Tools. In the **Applications and Services Logs** is the **Directory Services** logs (AD Logs). Should check these logs when going the promotion of a server to a DC
    a. Would do the same for the **DNS** and **DFS Replication logs**

![image 47](https://github.com/user-attachments/assets/8e5c95e0-b6d2-4081-b51a-3ad363d05f5c)

## Active Directory Operations Master

In AD there are **five operations Masters** 

1. In **Active Directory Users and Computers** of Server B19**,** right click the Domain Controller and select **Operations Master.** The main purpose of this is to transfer masters from one DC to another DC
    a. The following masters are found in AD Users and Computers: **RID, PDC, Infrastructure**
    b. There are 2 other Operations Masters

![image 48](https://github.com/user-attachments/assets/6362af9d-a4fd-433c-91a8-8e0663f38503)

![image 49](https://github.com/user-attachments/assets/faff0368-cddc-496a-b66e-8576e024ca25)

2. Go to **Active Directory Domains and Trusts**. Right click on **AD Domains and Trusts** and select **Operations Master…**

![image 50](https://github.com/user-attachments/assets/69e2a801-5410-44e8-8d3f-19a5e8f73771)

![image 51](https://github.com/user-attachments/assets/9dee8b27-605f-4af1-b9cb-283398b231c8)

3. At the forest level is the **Schema Master.** In **Run** type **mmc,** select **File → Add/Remove Snap in…** Notice there are only three AD services available. The **Schema is never visible by default**

![image 52](https://github.com/user-attachments/assets/34513ce4-e0f9-4746-9e5d-d12f1c54d901)

4. To view the schema, go to **Run,** and type **regsvr32 schmmgmt.dll.** Go back to **File → Add/Remove Snap in…** Now the Schema is available. Add it to the Snap Ins. Now the schema is visible

![image 53](https://github.com/user-attachments/assets/145a7347-0722-4de9-b383-e56c11251962)

![image 54](https://github.com/user-attachments/assets/53ca2a75-cb7e-4cb4-aa0a-30e93363f09b)

5. The purpose of a schema is to view what **objects (Classes)** can be created

![image 55](https://github.com/user-attachments/assets/1c911c8a-d44e-4f2c-8b7a-0111e1c15f1c)

  a.  **Attributes** would be properties of those objects

![image 56](https://github.com/user-attachments/assets/3f3cdf87-4f39-4aec-9474-e3c1567bc15d)

6. Right Click the Schema and select **Operations Master…**

![image 57](https://github.com/user-attachments/assets/097ef41d-8dfc-4dda-8939-a8cc786b7ec6)

![image 58](https://github.com/user-attachments/assets/78c6c77a-d77f-4d48-871a-95b3f2b3ad6b)

7. In **cmd** type **netdom query fsmo.** The Operations Masters are called **Flexible Single Master Operations.** Currently Server A19 holds all of the roles because by default the first DC created holds all the roles

![image 59](https://github.com/user-attachments/assets/20bba87e-7e5a-4e6c-8dc9-67dacd4ca20e)

It can be confirmed after doing checks In AD Users and Computers, Group Policy Management, Event Viewer, and Checking the Operations Masters, the replication process of Server A19 to B19 was done successfully

All previous steps were again, conducted in Server B19 (Windows 2019)

# Installing Windows Server 2022 for Server A22

1. Back in VirtualBox create a new VM for Windows Server 2025 and follow the same process done for Windows Server 2019 Servers A19 and B19 and name this server **A22 (Windows Server 2022)**

![image 60](https://github.com/user-attachments/assets/a40f4b51-1851-4e8e-9a70-ad4954bc45d1)

2. Follow the installation process for Server A22 as done with Server 2019 Servers

3. Change the Name of **Server A22** and the IP of the server as done previously with Servers A19 and B19. Ensure the **Preferred DNS** is pointing to Server **A19**’s IP address

![image 61](https://github.com/user-attachments/assets/d55cfb20-7c0d-48dd-910f-17070e91329e)

![image 62](https://github.com/user-attachments/assets/cfa36ecf-93a3-429d-89c4-c645a185e405)

4. In Dashboard, select **Add Roles and Features**. In the **Add Roles Wizard,** select **Next** until the **Server Role** tab then check the **Active Directory Domain Services** box. Continue Selecting next then press **Install**

![image 63](https://github.com/user-attachments/assets/2b0d1a66-bd26-4247-81d8-87aac967fe2b)

5. Once Install is completed, click the **Flag Icon** and begin the **Promotion Process** for Server A22. Under **Deployment Configurations,** select **Add a domain controller to an existing domain,** and ensure the domain and admin user login are provided for Server A19 in order to successfully replicate

![image 64](https://github.com/user-attachments/assets/bafad09b-13f0-40d6-87e1-e4fe79b5eeef)

6. Check the boxes to make Server A22 a DNS Server as well as a Global Catalog, then provide a DSMR password

![image 65](https://github.com/user-attachments/assets/4bbd9848-2590-47de-8083-f95e57796d16)

7. In **Additional Options** tab, make sure to select Server A19 to be **replicated** from. From there select Next for defaults and then **Install AD Domain Services**

![image 66](https://github.com/user-attachments/assets/0be42776-89f6-4cdc-8528-4748cf6e5425)

![image 67](https://github.com/user-attachments/assets/1ef4eab3-67bf-402a-affe-d2f531431a60)

8. Follow the same steps when checking if replication was completed correctly as done with Server B19

![image 68](https://github.com/user-attachments/assets/aa46cc4c-f26c-471d-b580-93e65ec5f2ab)

![image 69](https://github.com/user-attachments/assets/827d714d-367d-4636-adb6-eb124f5c038d)

![image 70](https://github.com/user-attachments/assets/e54bab80-7c46-4dce-9d90-3ea389d2b944)

This process was done for Server A22 (Windows Server 2022)

# Demoting Windows Server 2019 Domain Controller (Server B19)

1. Before demoting Server B19, always ensure the DC you’re about to demote is not an Operations Master by running the **netdom query fsmo** command. Currently all operations masters are on Server A19. In **cmd** type **ipconfig/all.** 

Always ensure none of the DNS Servers use the **Preferred IPv4 Address of Server B19 (172.16.1.101).** When demoting the server from being a DC, it will also **remove DNS,** so ensure no machines are pointing to that DNS or IP

![image 71](https://github.com/user-attachments/assets/26b4688f-4ade-464b-a472-786962a4b589)

2. Begin the demotion process in Server B19 by going **Manage** in Server Manager and selecting **Remove Roles and Features.** Select the Server being demoted (B19) and move on

3. Under **Server Roles,** select **Active Directory Domain Services**

![image 72](https://github.com/user-attachments/assets/7dac85bd-eb25-4cd7-80a6-7499b4f99259)

![image 73](https://github.com/user-attachments/assets/bc10428a-939a-47ab-8a55-52e55b15446c)

4. An error message will pop up because you have to **Demote the Domain Controller before it can be removed,** so within the error pop up, select **Demote this domain controller**

![image 74](https://github.com/user-attachments/assets/e02989ef-3708-45ac-9741-e9fa9d60788b)

5. In the **Credentials** tab, provide credentials if not an admin, but if so, they will be input automatically.
    
    **Force the removal of this domain controller** check box should not be selected unless that is the **last** DC in the entire domain meaning everything would be torn down. In this case, it won’t be selected
    

![image 75](https://github.com/user-attachments/assets/ef5bce06-33de-4d57-bf6c-3437ff2903fb)

6. In the **Warnings** tab, it will notify that Server B19 will no longer be a DNS Server nor a Global Catalog server

![image 76](https://github.com/user-attachments/assets/b30d4b89-b6c9-4a9f-9bb2-dcba8f40672b)

7. **New Administrator Password** sets this back to be a **member** server of the domain. Doesn’t kick the server from the domain. Once password is input, in the **Review Options** tab, select **Demote**

![image 77](https://github.com/user-attachments/assets/55ec5c17-ef24-4e47-8a99-5809e64790c1)

![image 78](https://github.com/user-attachments/assets/a6df7eb3-6dd1-4218-be0f-d637e985cdfb)

8. Once Demotion is selected and verified, the Server will restart

![image 79](https://github.com/user-attachments/assets/d79b7bfd-cede-4a23-9a31-c7bb33543d10)

Ensure to sign into the **Domain (domain\user)** and not the **Local (user).** All of the AD roles in tools will still show, but the demotion process was completed

![image 80](https://github.com/user-attachments/assets/03cf8003-a008-4f0f-8834-cf7452f49e18)

9. Navigate to **Active Directory Users and Computers** on Server B19. Select the **Domain Controllers** OU and both domains A19 and A22 can be seen, but not B19 since its been demoted. However, in the **Computers** OU, Server B19 can be seen

![image 81](https://github.com/user-attachments/assets/da464759-0a1f-4a4c-8994-3709b6052724)

![image 82](https://github.com/user-attachments/assets/e94ea009-a73a-44ac-a91b-6fc850a6932e)

![image 83](https://github.com/user-attachments/assets/7e92ae6e-ff84-43ba-8060-dae65bbe5f3f)

10. Right click the domain and look at the **Change Domain Controller…** options. Both DCs A19 and A22 will be able to be remotely connected to
12. Go back to Server Manager → Manage → **Remove Roles and Features.** It can be seen that **AD Domain Services** is still checked on the list, but earlier it was selected. That is because the Domain Controller had to be **demoted** first and only then can service be removed.

![image 84](https://github.com/user-attachments/assets/65857c53-67fd-4bf2-a955-55f43b1b56af)

13. Uncheck both, **AD Domain Services** and **DNS Server.** Select Next for other tabs and in the **Confirmation** tab, select **Remove,** then log back in after reboot
    
    

![image 85](https://github.com/user-attachments/assets/a4b7cc5c-2110-4005-ba12-a01bfd6cc4f4)

![image 86](https://github.com/user-attachments/assets/29f9b8c3-d729-43b1-a145-d25a73ea030b)

The demotion process for Server B19 (Windows Server 2019) was successfully completed and can now be used a basic server within the domain or removed.

## Removing Server From Domain (Server B19)

1. If an admin doesn’t even want the server within the domain, they can navigate to Server Manager → Local Server →  Domain → Change → Select the **Workgroup** radio button and input workgroup for server

![image 87](https://github.com/user-attachments/assets/5c13a9b9-d027-4f62-a167-7926e2f9be3c)

![image 88](https://github.com/user-attachments/assets/4c8d4005-1e2f-4f21-af93-fbaf9f147208)

2. The other option that can be taken is if admins are certain the server is no longer needed, they can completely decommission the server in VirtualBox

![image 89](https://github.com/user-attachments/assets/93e3bba5-650d-48c4-b1b8-f521edab5e47)

These steps followed the demotion and decommission process for server B19 (Windows Server 2019). The only remaining servers in the currently are: Servers A19, A22,.

## Observe Changes in Server A22

The next steps will follow the process for Server A22(Windows Server 2022)

1. Now that Server B19 has been decommissioned and disjoined from the Domain. In Server A22, to confirm so, navigate to **AD Users and Computers,** and in the **Computers** container it can be seen that the computer for B19 has a down arrow icon meaning the computer account has been disabled

![image 90](https://github.com/user-attachments/assets/dc347113-c517-454e-83bb-48697fb0d75e)

2. The computer is not going to be rejoined or connected to the domain again, so it can be deleted

![image 91](https://github.com/user-attachments/assets/2de48351-bad0-40e5-b3bf-91423a1de110)

3. Go to **AD Sites and Services,** and it can be seen in **Default-First-Site-Name → Servers** Folder that B19 is still there, although nothing is in it, it is good practice to delete the Server from this folder to avoid confusion for the next admin

![image 92](https://github.com/user-attachments/assets/db6946e5-dcb7-4100-ac8e-02ffdd849f20)

At this point in the environment, it is running a mixed domain environment. Server A19 (Windows Server 2019) and Server A22 (Windows Server 2022)

# Provision Windows Server 2022 on Server B22

Now, Server B22 (Windows 2022) build process will begin and all steps will take place in Server B22 

1. Follow the same steps for creating a new Windows Server VM and also the same steps when installing Active Directory and Promoting the server. Once done, Server B22 (Windows Server 2022) will be established and Server A19 will be ready for demotion and ensure to replicate credentials from Server A22 since A19 will be demoted shortly

![image 93](https://github.com/user-attachments/assets/e5350a94-8871-4440-9098-7791e8fd8fcf)

2. Be sure to give the IP Address of B22 172.16.1.201 and have the preferred DNS be the IP from Server A19 172.16.1.100. Ensure communications and promotion can be done by pinging Server A22
    
    ![image 94](https://github.com/user-attachments/assets/6a0b658e-3dfa-4ee5-a23e-61ba13cf5f70)


3. Confirm Replication was done correctly by checking Users and Computers, Sites and Services, and Group Policy Manager to see if all created OUs and other Servers are within the folders

![image 95](https://github.com/user-attachments/assets/5411c450-1ccc-4d7a-bd2a-20576ebf4aba)

![image 96](https://github.com/user-attachments/assets/a885eb92-0d63-42f1-8578-dc5fd03ccbf9)

![image 97](https://github.com/user-attachments/assets/32bd98dc-217c-48ca-9c9b-f73943aeae39)

During this process, Server B22 (Windows Server 2022) was established in VirtualBox and successfully promoted and provisioned as a Domain Controller in AD.

# FSMO Transfer and Demotion of Windows Server 2019 (Server A19)

The demotion process for Server A19 (Windows Server 2019) will now begin, and the following steps will take place in Servers A22/B22 (mainly B22 unless specified otherwise in the steps)

1. In Server A19 the IP Address is 172.16.1.100. It is important no users or machines are using or trying to access this Server’s IP during the demotion process
2. In Server A22, make sure that the server isn’t using A19 as a preferred DNS by right clicking **Ethernet → Properties → IPv4 .** It can be seen that Server A22 is using Server A19’s IP as **preferred DNS Server.** 

![image 98](https://github.com/user-attachments/assets/362064c5-75a6-433c-bf27-112964dcd24f)

![image 99](https://github.com/user-attachments/assets/2173d96e-5033-4387-a811-907ab54add35)

3. Change the **Preferred DNS Server** for Server A22 ****to **172.16.1.201** which is **Server B22**’s IP

4. To transfer the Operations Masters, go to **Server B22** and in **cmd** run **netdom query fsmo,** and it can be seen that A19 holds all five

![image 100](https://github.com/user-attachments/assets/c07be4f5-1c19-416b-9c12-f2a7e2fa79f3)

5. In Server B22, go to **AD Users and Computers** and right click the **Domain,** then select **Operations Masters.** 
    
    It shows that the current Operations Master for Users and Computers is Server A19. Select **Change…** and the Operations Masters will then be transferred to Server B22. Make sure to do this for all three roles in **AD Users and Computers** (RID, PDC, Infrastructure)
    

![image 101](https://github.com/user-attachments/assets/86d8d518-94d5-41e3-945c-d6332940a195)

6. In **cmd** of Server B22 ****using netdom, it can be seen that 3/5 of the FSMOs have been changed

![image 102](https://github.com/user-attachments/assets/987f8d41-77aa-44cf-a27a-81043cfc7da8)

7. For Server B22, in **Active Directory Domains and Trusts,** you can also make different DCs Operations Masters for specific Resources. Right click **Active Directory Domains and Trusts → Change Active Directory Domain Controller… →** Select Server A22.

![image 103](https://github.com/user-attachments/assets/f73f3820-5022-418d-9b1c-8a874bd5755b)

![image 104](https://github.com/user-attachments/assets/2c5bd694-d122-4e0e-836e-445e7aa0ab98)

8. Now right click **Active Directory Domains and Trusts** and select **Operations Master…** now the option to change the OM to server A22 instead of B22 is available and can successfully be changed from Server A19 to Server A22

![image 105](https://github.com/user-attachments/assets/00b7f1b6-1a45-4866-8b11-d61177289527)

![image 106](https://github.com/user-attachments/assets/e233e041-b9c1-4f2e-9b8c-573465249d92)

![image 107](https://github.com/user-attachments/assets/5f980de8-aefa-4ac3-ab3e-461bbe2ac2c1)

9. In **cmd** type **regsvr32 schmmgmt.dll** to activate the **AD Schema** and so it can be viewed. ****

![image 108](https://github.com/user-attachments/assets/ef5fbb7d-a013-4519-82b5-e2ac10ada818)

10. In **Run** open the **mmc** console. In Console Root, select **File → Add/Remove Snap in…** Select **Active Directory Schema** and Add.

![image 109](https://github.com/user-attachments/assets/01e81465-7fd4-4b43-9872-45aef12b20bd)

11. Right click **Active Directory Schema** and select **Change Active Directory Domain Controller…** Select Server A22, then right click the Schema and change **Operations Master** to Server A22

![image 110](https://github.com/user-attachments/assets/1f8dab3b-cf2a-44db-90d1-0c2022114424)

![image 111](https://github.com/user-attachments/assets/6cbd1a56-c8d5-4f13-a003-d62092bf2690)



![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/c229417d-4328-4af9-85e6-5beb2a21e082/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/7d2dbab6-b390-4c86-a3a4-993fcacc0430/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/87ec98d2-8419-4acc-8bee-8fc7f32ea7d4/image.png)

1. Confirm the Operations Masters have been changed in **cmd** using netdom, and as can be seen, the changes were successful

![Screenshot (509).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/e386c2a9-482e-42e7-ad34-435d2eac654e/Screenshot_(509).png)

At this point, there are now two 2022 Domain Controllers (Servers A22/B22) with the FSMO roles having been transferred to one another along with a 2019 Domain Controller (Server A19) with the previous being decommissioned. All that’s left is to demote the 2019 DC

To avoid confusion, steps **2** and **3** were conducted in Server A22. Steps **4-12** were all conducted in Server B22 only.

# Demoting The Last Windows Server 2019 Domain Controller

The final process will conclude the demotion and decommission process of Server A19 (Windows Server 2019)

1. Before Demoting Server A19, remember that Server B22 has A19’s IP as its preferred DNS, so  in Server B22, change the preferred DNS of B22 to A22’s IP (172.16.1.200). 

![Screenshot (510).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/8405f034-31aa-4163-86bd-0c80d0fb0af8/Screenshot_(510).png)

1. Switch to Server A19, begin the demotion process by selecting **Manage** in Server Manager and choosing **Remove roles and features.** Follow the same process as when demoting **Server B19**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/f12cb4b2-b275-43ec-a015-b239ad9bc20e/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/5c42170c-d0a2-460a-b441-7386e0d8581a/image.png)

1. Once Server A19 is demoted, it can be treated as a local server or decommissioned. Verify that both Servers A22 and B22 have been set to your domain. If it says **Public** or **Identifying,** select **Change Adapter Settings** and Rick click the **Ethernet**, select **Disable** then select **Enable**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/1fcf8002-b18a-44d7-8614-f56d93c242bb/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/ab7fee23-5846-4865-b0c9-fce8292027b1/image.png)

# Domain Functional Level

Once all Domain Controllers runs a certain OS or higher, the **Domain Functional Level** can be risen. In this case, since Servers A19/B19 (Windows Server 2019) have been decommissioned, and the remaining Servers A22/B22 (Windows Server 2022) are in place; the functional level can be risen

1. In **Active Directory Users and Computers** for both Servers A22/B22**,** right click the domain and select **Raise domain functional level…** The highest functional level for Windows Server 2019/2022 would be Windows 2016

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/c00d403a-2af0-4337-9b79-221e762ebd7a/image.png)

1. To raise the **Forest** functional level, go to **AD Domains and Trusts** in either Server A22 or B22**,** right click **Active Directory Domains and Trusts,** and select **Raise Forest Functional Level**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/246692bd-cf92-4f91-8b4e-6d4c15c8bf7a/dfbd46f0-e9e9-4015-81ef-ccc3832d14f9/image.png)
