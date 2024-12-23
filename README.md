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





















