# AD-Lab

## Index
- [Introduction](#introduction)
- [Setup VMs](#setup-vms)
    

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
- Lets open your browser and search for ubuntu.com. Go to product and click on ubuntu servers. Click on Download Ubuntu Server 

