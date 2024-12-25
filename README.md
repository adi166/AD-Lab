# AD-Lab

## Index
- [Introduction](#introduction)
- [Setup VMs](#setup-vms)
-   

## Introduction
The primary objective of this project is to design and implement an Active Directory environment integrated with Splunk for comprehensive event monitoring and analysis. By simulating a brute force attack using Kali Linux and conducting additional testing with Atomic Red Team, the project aims to provide hands-on experience in setting up and managing IT infrastructure, detecting potential threats, and analyzing system events. This initiative is intended to enhance skills in IT administration, event telemetry, threat detection, and response, equipping participants with practical knowledge for real-world cybersecurity challenges.

![image](https://github.com/user-attachments/assets/228a5aff-d44a-4f7d-8149-c9f726e0431b)

The diagram illustrates the network setup for our project:

- Windows Server: This server will run Active Directory.
- Windows 10 Machine: This will be the target machine for the Active Directory.
- Kali Linux Machine: This machine will be used to perform brute force attacks and other security testing.
- Splunk Server: This will be running on an Ubuntu server to collect and analyze logs and events from the Windows Server and Windows 10 machine.
- Network Infrastructure: Includes a router and a switch to connect all devices to the Internet and each other.

## Setup VMs
We have already set up our Windows 10 and Kali Linux in our <a href="https://github.com/adi166/HOME-LAB">Home-Lab</a>. Now we only need to set up our 3 other Machines Windows 10 Server, Splunk Server, Active Directory Server.

### Install Windows Server
- Go to google in your main machine and serach for Windows server 2022 iso. Using this <a href="https://www.microsoft.com/en-in/evalcenter/download-windows-server-2022">link</a> you could directly land on the download page.
- After downloading the iso file we would now install it on our VirtualBox. Now Open VirtualBox -> Click New icon -> Type Name, select folder, select iso file -> assign RAM and storage according to you -> Click on finish

  ![image](https://github.com/user-attachments/assets/a9dadcc4-fd7b-4710-ac2e-8b888e62f7c7)

- Run your VM and install Windows Server. An option would appear to install which type of interface you want, Select the 2nd interface because it is a GUI interface and its easy to navigate. And click on Custom Install.

  ![image](https://github.com/user-attachments/assets/6a31afd4-e317-44f9-87b2-85fec1a19c43)

  ![image](https://github.com/user-attachments/assets/baf3de3d-1293-436c-92c2-e05d2c6b7bc9)

- Now we would add passowrd our account and now we would see our Windows Server Manager.

  ![image](https://github.com/user-attachments/assets/838f755f-a928-4e1f-9fc0-fa68e8255a4a)

### Install Splunk Server
- Lets open your browser and search for ubuntu.com. Go to product and click on ubuntu servers. Click on Download Ubuntu Server.
- Follow the above steps to setting up Ubuntu Server on VirtualBox.

  ![image](https://github.com/user-attachments/assets/81712a36-165b-4a82-ac33-738714cc3f1c)

- For the hardware,  set the ram to 8 GB and processor to 4. You may want to make the Splunk machine a little better in terms of hardware compared to other machines because this is going to be ingesting data and running searches on it. Set the hard disk to 100 GB and finish the installation.

  ![image](https://github.com/user-attachments/assets/8a4d2de2-0cd0-49c9-8cfc-ec7fa99814bd)

  ![image](https://github.com/user-attachments/assets/4ae5630f-ed6f-4d0d-a6ba-f681ce396b97)

  ![image](https://github.com/user-attachments/assets/33d242db-5041-4a68-bb1d-a3534243f38e)

  ![image](https://github.com/user-attachments/assets/612f7ad6-a17f-44cf-912a-1602492e02a1)

  ![image](https://github.com/user-attachments/assets/a8b94ed6-a716-4d18-9ce2-7b654566fa18)

  ![image](https://github.com/user-attachments/assets/5253434d-63dc-4825-9519-15b2e41339d8)

  ![image](https://github.com/user-attachments/assets/ff20937c-111e-4781-bcdf-64a3f2cdd16e)

  ![image](https://github.com/user-attachments/assets/34ce6d04-ec9c-4dee-95cd-396ee42ccc52)

  ![image](https://github.com/user-attachments/assets/5617dbb8-8225-46b2-be39-de0cf9d78690)
    
  ![image](https://github.com/user-attachments/assets/d2bec694-09b4-41db-aa6a-5bc1bb8ab4a2)

  ![image](https://github.com/user-attachments/assets/90ec2a44-051c-441c-9189-20e4e3d761f4)

  ![image](https://github.com/user-attachments/assets/405da4ce-6e6e-4bd1-86eb-aab6d280d419)

## Installation of Software
Before we install our main software splunk and sysmon, we need to setup our Network in VirtualBox so that all VMs can communicate with each other and have same internet access. On VirtualBox go to tools -> Network -> Click on NAT Network. Then click create. In general options we are going to enter the name and IP address that we have previously decided.

![image](https://github.com/user-attachments/assets/57450ca7-a706-49fb-8bb4-0b51814f32d0)

Next we are going to connect all the VMs to the network we have just created. To connect we select the VM and then go to its Settings -> Network and select the "Attached to" option as "NAT Network", select the network name. Follow this step for all the other VMs.

![image](https://github.com/user-attachments/assets/0e0b9b87-cf4c-4557-8947-ed8f7f0fbeea)

Now in our diagram we have given splunk a static ip address of 192.168.10.10. In order to do that we need to `sudo nano /etc/netplan/00-installer-config.yaml`. Follow the following and setup everything accordingly.

![image](https://github.com/user-attachments/assets/f74eb84b-f03c-4841-9fd3-8b0451fc2d15)

After saving and exiting with Ctrl + X and Enter, write ` sudo netplan apply `, and after we check our IP with the command `ip a` and ping google.com to confirm we have the correct IP and also a connection.

### Splunk

On our host machine we are going to install Splunk file. To do this go to splunk website, login/register, go to free trials in Product section. We want to install .deb file, because we are using Linux server.

![image](https://github.com/user-attachments/assets/b7a8663e-d782-4e39-afd7-fa2403bf9d81)

Next step is to install Guest Addons for VirtualBox. ` sudo apt-get install virtualbox-guest-additions-iso `, Then we click on the shared folder settings, And click on the "Add a new shared folder" button. Here, the folder that we want to select is the one where the Splunk .deb installation file resides. Select all options: Read-Only, Auto-mount, and Make-Permanent, and click OK.And reboot the VM with `sudo reboot`.

![image](https://github.com/user-attachments/assets/a8fc043d-1e12-4a81-aa1e-2d01f285e1d0)

Now we nned to add a user to the vboxsf group. Follow by typring `sudo adduser "yourusername" vboxsf`. If there is an error you may need to add additional installation that virtualbox offers and reboot after installation.

![image](https://github.com/user-attachments/assets/e60859a6-3d5e-47ff-9afb-bbc148141c06)

Now you would be able to add the user. 

Next step is to create a new directory: `mkdir share`. After creating the directory 'share', we want to run the following command to mount our shared folder on the directory called 'share': `sudo mount -t vboxsf -o uid=1000,gid=1000 dL share/`. After this step, you will now be able to see the 'share' folder as highlighted.

![image](https://github.com/user-attachments/assets/56393a4c-4557-4cdc-82ce-2ecb37dd74ff)

Now you can go into this directory and run the installation command: `sudo dpkg -i splunk` and tab to complete the Splunk file.

![image](https://github.com/user-attachments/assets/8337160e-d12c-46e7-95d5-18ba61d23b7e)

After the installation is complete, we go to the Splunk directory with `cd /opt/splunk/`.

![image](https://github.com/user-attachments/assets/4f1efdb9-b276-453d-af6b-cab394d23edf)

You will notice that most of the users and group are splunk i.e it belongs to splunk, which is good because it limits permission to that user. Let's change into the user Splunk by typing `sudo -u splunk bash`.
Now we are acting as the user splunk. Change into the directory called 'bin' as the files listed here are all binaries that Splunk can use. Here we run the command `./splunk start` to run the installer. Accepting the agreement and then creating a username and password, our installation will be completed. The Splunk web interface is at http://splunk:8000 or http://"SplunkIPAddress":8000.

Let's run one more command to make sure that our Splunk starts up every time our virtual machine reboots. Type 'exit' to exit out of the user splunk first. Then go to the 'bin' directory again under the main user, whatever it is for you. And run: `sudo ./splunk enable boot-start -user splunk`. This will make it so that anytime the VM reboots, Splunk will run with the user splunk.

![image](https://github.com/user-attachments/assets/343a825a-3fd5-40e5-a99c-54af67e58b7a)

### Splunk Forwarder and Sysmon
Now we have to install the Splunk Universal Forwarder and Sysmon on both our target machine and our server.

First, let's start with renaming our PC to Target-PC. After restarting our target PC, we first check our IP address.

We do not have to change the IP address of the target machine since there is no IP conflict per our diagram.

![image](https://github.com/user-attachments/assets/d54766d3-e2c8-4e07-a217-63e9456fdb69)

We do not have to change the IP address of the target machine since there is no IP conflict per our diagram.

Now let's open a browser window and try to connect to our Splunk server. Our Splunk IP address was 192.168.10.10, and we know that Splunk listens to port 8000:

![image](https://github.com/user-attachments/assets/6c9d5af8-477f-4035-8b65-92010a85ad24)

And as you can see, we can connect to its login page with no problem. Now let's install the Splunk Universal Forwarder on this machine so we can forward our logs to the Splunk server. Go to the Splunk website and log in with the account that you created. And download the Universal Forwarder for Windows.

![image](https://github.com/user-attachments/assets/86617b1e-5e07-4e12-9562-db61435a80fd)

After the download is complete, start the setup.

![image](https://github.com/user-attachments/assets/1f8c7fd5-7cfc-4a8b-b270-78ad67faabd5)

On the next page, for the username, you can put anything you like. I will put "Admin" and clicked the "Generate random password". On the next page, for the deployment server, just skip without entering anything because we don't have one.

On the next page, which is the Indexer page, here we will enter our Splunk server's IP and put the port as 9997, which is the default port for the Splunk indexer. Then click "Next" and start the installation.

![image](https://github.com/user-attachments/assets/c391ce75-cb9f-4fed-a1ed-9d21da76a64b)

Meanwhile, lets download sysmon, so go to <a href ="https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon"> Sysmon - Sysinternals | Microsoft Learn </a> and download Sysmon. The Sysmon configuration that we will be using is Olaf. So go to the <a href="https://github.com/olafhartong/sysmon-modular">GitHub - olafhartong/sysmon-modular: A repository of sysmon configuration modules </a> and download the `sysmonconfig.xml`.

![image](https://github.com/user-attachments/assets/f2bc4654-d327-4bd6-88e7-36f8bc471817)

You can click "Raw" and right-click "Save as" and download the file. Next, go to the downloads directory and extract Sysmon from the zip file.

![image](https://github.com/user-attachments/assets/e8450122-d931-49dc-89b1-242a55d69d3e)

Open up a PowerShell in administrator mode and go to the location of this directory. You can copy the location from the menu bar and paste it into the PowerShell with the cd command. To install Sysmon, run the command `.\Sysmon64.exe -i` with a configuration file. And since the config file that we installed is in the downloads folder, we can run this command: `.\Sysmon64.exe -i ..\sysmonconfig.xml`

![image](https://github.com/user-attachments/assets/c7ce2073-3965-4f95-a192-7d9294a295ab)

And Sysmon will be installed shortly. And meanwhile, our Splunk Universal Forwarder is also installed.

Now is the most important part. We need to instruct our Splunk Forwarder on what we want to send over to our Splunk server. To do this, we must configure a file called input.conf. This file is located at `C:\Program Files\SplunkUniversalForwarder\etc\system\default`
But we will not edit the `input.conf` file under the default directory because the configs in this directory are there just in case we mess anything up, so we will create a new file under the local directory.

Now, to be able to create a new file in the local directory, we need to be an administrator. So to do this, we will search for Notepad from the search bar and open it as an administrator.

```
[WinEventLog://Application]
index = endpoint
disabled = false

[WinEventLog://Security]
index = endpoint
disabled = false

[WinEventLog://System]
index = endpoint
disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = endpoint
disabled = false
renderXml = true
source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```
You can copy and paste this into the new Notepad that you opened. This is instructing our Splunk Forwarder to push events related to the application, security, system, and also Sysmon to our Splunk server.

Important note: See that the index that we are pointing to is index = endpoint. This is important because whatever events fall under these categories will be sent over to Splunk and placed under the index "endpoint". If our Splunk server does not have an index named "endpoint", it will not receive any of these events.

After filling in the config file, save it under the local directory as we mentioned before as inputs.conf. `C:\Program Files\SplunkUniversalForwarder\etc\system\local`

![image](https://github.com/user-attachments/assets/28f461d2-e99e-4dbe-955c-7d65fc328ee2)

Do note that anytime you update the inputs.conf file, you must restart the Splunk Universal Forwarder service. To do this, we open the Services as an administrator and find the "SplunkForwarder".

Another important note: If we see that the "Log on as" line is "NT SERVICE":

![image](https://github.com/user-attachments/assets/cbd291c0-4578-42fa-9a19-509598076bd4)

Go on and double-click it, and look at the "Log On" tab from the menu:

If you see that it is using "This account" and it is "NT SERVICE\SplunkForwarder", it might not be able to collect some of the logs due to some of the permissions. So you want to select "Local System account" and hit "Apply".

![image](https://github.com/user-attachments/assets/888f0f44-d8a6-45db-a24d-486569c8f932)

Which then will ask us to stop and restart the service, and we will do so. Just double-check on the Services menu that SplunkForwarder is "Log on as" Local System and also that Sysmon64 is running.

After finishing these steps, we can finalize our Splunk server configuration. Head over to "http://192.168.10.10:8000/" and log in with the credentials that we set during the Splunk server installation.

On the menu, we want to go to the Indexes from the Settings menu. Now recall that our index name was "endpoint" from the Splunk Universal Forwarder `inputs.conf file`. We now will create this index. Just click on the "New Index" button from the top right and enter the name "endpoint" and save.

![image](https://github.com/user-attachments/assets/2263bf0f-3ca2-4dfb-b389-253405aaa24f)

Now you will be able to see the "endpoint" index on the list. Next, we need to make sure that we enable our Splunk server to receive the data. In order to do that, go to Settings > Forwarding and receiving and under the "Receive data", click on the Configure receiving button, and there click on the New Receiving Port on the top right, and recall that during the setup, we mentioned that the default port is 9997, enter it and hit save.

Now at this point, if we set up everything correctly, we should start seeing data coming in from our target machine. Click on the Apps from the top left corner, and select Search & Reporting. And here on the search bar, write index="endpoint" and click enter.

![image](https://github.com/user-attachments/assets/20e14130-1326-4a81-8598-d3db57c75225)

And we do see some events, and if we scroll down a little bit and see the "host" field from the left, we see that it is TARGET-PC. We also see some sources and sourcetypes, which is exactly what we entered in our input.conf file on the Target-PC.

![image](https://github.com/user-attachments/assets/59270892-2d52-4cc4-a266-ee6ee357781b)

Now it is time to do this same setup on our Active Directory (Windows Server). Also, change the name of the Windows Server machine to ADDC01. And change the IP address of the machine to the value on the diagram.

![image](https://github.com/user-attachments/assets/53b7704d-c4dc-4d39-9802-c17d61fa98c8)

After this all we need to do is follow the same steps to install both splunk and sysmon so I am not going to show this part. If we have followed all the steps successfully, we would be seeing two hosts.

![image](https://github.com/user-attachments/assets/0d29d672-503a-48df-b508-fdc9b13489f7)

## Install and Configure Active Directory
Now we need to install Active Directory onto our server and then promote it to a Domain Controller. And then we will be configuring the target machine to join this domain. Double-check the IP address of the Windows Server using `ipconfig` command in cmd, and then open Server Manager and on the top right go to Manager > Add Roles and Features.

![image](https://github.com/user-attachments/assets/90602133-22d0-4c46-8e03-af4e8739e560)

Click "Next" and on the next page, select "Role-based or feature-based installation".

![image](https://github.com/user-attachments/assets/eab18e16-3fec-42e4-a92b-e83204e89f7e)

Next, you will select the server from the list, since we only have one server, there will be only one on the list, and we select and go "Next".

![image](https://github.com/user-attachments/assets/35c582c2-0733-4b9e-9184-2fe460f49163)

On the next page, we will set the Server Roles. Here we want to select the Active Directory Domain Services.

![image](https://github.com/user-attachments/assets/088ebf90-ce17-4a5e-915f-6b0c5e9fe2dc)

Click on the "Add Features" on the pop-up page and go "Next".

![image](https://github.com/user-attachments/assets/4644bd3e-2a4a-4b82-807e-1e30f620e359)

From this point, click on "Next" until you get to the "Install" step, and start installing.
Click on "Close" when you see this "Installation succeeded" message.


![image](https://github.com/user-attachments/assets/9295064a-01a3-4cdb-95f5-f780a247787f)

On the top right, click on the Flag with the yellow sign and click on "Promote this server to a domain controller".

![image](https://github.com/user-attachments/assets/ebcf2021-bebc-4673-9c12-eace6f8bb5e5)

And next, select the "Add a new forest" option because we are creating a brand new domain.

![image](https://github.com/user-attachments/assets/50af48b2-8e78-4bb8-b695-3d077557e176)

And next, select the "Add a new forest" option because we are creating a brand new domain. 

![image](https://github.com/user-attachments/assets/81ba4bac-4808-4e0b-8784-86710286b834)

Now, as a domain name, we will add it as "addLAB.local". The domain name must have a top-level domain, so in other words, the domain name cannot be just "addLAB", it must be "addLAB.something". On the next page, leave everything as it is and create a password, and from this point, just click "Next" for the upcoming pages.

![image](https://github.com/user-attachments/assets/a4c30fdb-b1b4-4c23-90e8-115fe3c5bbc2)

Quick note for this page: These are the paths used to store our database file named "NTDS.dit", and for your information, attackers love to target domain controllers, not only because it has access to everything but also because of this file as it contains everything related to Active Directory, including password hashes. If you do notice any unauthorized activity towards this file, you can assume that your entire domain is unfortunately compromised. Click "Next" a couple more times, and after all prerequisite checks have passed, click on "Install".

Once the setup is completed, the server will automatically restart. And we will see our domain followed by a backslash, which indicates that we successfully installed ADDS and promoted our server to a domain controller. Next step is to start creating some users, so let's log in and do just that.

On our server manager, we want to click on the Tools at the top right corner and select Active Directory Users and Computers.

![image](https://github.com/user-attachments/assets/40a0f66c-563d-4acc-8b39-0deaccd8cfca)

And if we click on the "Builtin", we see all of the groups listed on the right side. These groups have all been automatically created by the AD, and we can double-click any of these and read the description.

![image](https://github.com/user-attachments/assets/1e7a5c79-fd33-4f3e-837e-b7f371b46337)

And under the "Members" tab, we will see who is assigned to this group, and under the "Member Of" tab, we will see what groups this group is in. Note: You cannot add additional groups within a built-in group, but you can create a custom group and add that built-in group to that custom group.

Now if we click on the "Users" folder, we see different users and groups in here. You can right-click and create a group or user here, but in a real-world environment, it is likely broken up into different departments, AKA organizational units, for example, HR, Finance, IT, Sales...

To mimic this, we can right-click on the domain and go under New > Organizational Unit and name it, for example, "IT", and click OK, and now you have a new organizational unit created.

![image](https://github.com/user-attachments/assets/b1704dfc-7984-4ed3-b2f9-6a02f5cf4226)

Under this "IT" organizational unit, right-click and create a new user, and put a name and username.

Click "Next" and create a password, and finish.

And let's create another organizational unit and create another user under it.

![image](https://github.com/user-attachments/assets/215b2c78-e07f-407b-af6a-06432e118843)

There are many scripts out there that can help auto-create users, groups, and computers, but for this project, it is not necessary.

Now that we have our ADDC, we will now head over to our Windows Target-PC and join it to our newly created domain called "addLAB.local" and authenticate using the "Anil Kumar" account.

Power up our Windows 10 machine and search for PC > Properties > Advanced system settings.

![image](https://github.com/user-attachments/assets/91a2e3b4-e8b0-484f-b2d1-db4df7211c21)

Under the "Computer Name" tab, click "Change" and select "Member of Domain", and write our domain name "addLAB.local".

![image](https://github.com/user-attachments/assets/259378f3-f521-4afd-bf27-6db8bdf11b8c)

When we click OK, we might get an error saying the domain could not be contacted. This is because the machine does not know how to resolve our domain, "adlab.local", and this is how DNS works.

![image](https://github.com/user-attachments/assets/41dbca15-af4c-453a-9892-4130048f3ce0)

To fix this, go to the network adapter > change adapter options > select the adapter and right-click and go to properties and change the DNS server to our ADDC IP.

![image](https://github.com/user-attachments/assets/4b9cd54e-12f1-46c4-babb-3fa5ec6a7431)

And to check the configuration is set, open a command prompt and run `ipconfig /all`.

![image](https://github.com/user-attachments/assets/744702ff-9325-4ce8-a93a-f8ad48534bb3)

Now let's try again to join our domain. And now we are prompted to log in with some credentials.

![image](https://github.com/user-attachments/assets/b8135091-e70a-4bf2-9b55-9b795f21bbe4)

![image](https://github.com/user-attachments/assets/1f856cd0-2011-4967-8cf2-fa393968a255)

In a real-world environment, you would create users and put them into a custom group that is authorized to allow computers to join the domain.

You will be prompted to restart the computer, and once we are on the log-on screen, we want to log in with the "Anil Kumar" account. In order to do so, we want to click on the "Other user" and make sure that our "Sign in to" is pointing to our domain, and we see that it is "ADLAB", so we are good.

![image](https://github.com/user-attachments/assets/3e6f866f-dcd0-45ad-bc06-60d126bc84bb)

After you log in, you might see that some of the usual properties and sections are not accessible, like the PC name. This is because right now this PC belongs to the ADLAB domain, and there are rules in place.




































