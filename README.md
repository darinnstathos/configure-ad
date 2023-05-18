<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

<h4>- Step 1</h4><p>Set up Azure Resources: Domain Controller, Client-1 (Windows VM), setting Domain Controller's NIC Private IP to static</p>
  <h4>- Step 2</h4><p>Ensure & Establish Connectivity between the Client & Domain Controller</p>
  <h4>- Step 3</h4><p>Install Active Directory</p>
  <h4>- Step 4</h4><p>Create an Admin account and normal user account in Active Directory
  <h4>- Step 5</h4><p>Join Client-1 (Windows VM) to the domain
  <h4>- Step 6</h4><p>Setup and Establish Remote Desktop for non-administrative users on Client-1</p>
  <h4>- Step 7</h4><p>Create additional users and log into Client-1 with those users</p>


<h2>Deployment and Configuration Steps</h2>

<p>
  <h3>Step 1: Set Up Resources in Azure</h3>
  <p>In step 1, we’re going to create a Domain Controller, a client (Microsoft VM), and set the Domain Controller’s NIC Private IP Address to be static.</p>
  
  <h4>Creating the Domain Controller:</h4>
  <strong>What is a domain controller?</strong>
  <p>A domain controller is a server that manages and authenticates user access to a network domain. Domain controllers store user account information such as usernames and passwords and controls access to network resources. They help authorize and authenticate users, facilitate efficient and streamlined network administration, and improve overall security.</P>
  
  1. Naviagte to Microsoft Azure and select 'Virtual Machines'
  2. Within the Virtual Machines creation portal, create a Resource Group to hold both the Domain Controller and Client. In this instance, we'll name it: "AD-Lab" (ActiveDirectory-Lab)
  3. We'll create the Virtual Machine Name: "DC-1" (DomainController-1)
  4. Select the image: Windows Server 2022 Database Azure Edition - x64 Gen2
  5. Size: 2vcpus is recommended as it allows for the system to still run efficiently without eating up too many resources
  6. Create a username/password. In this example, I used my personal name: darinstathos
  7. Take note that Azure automatically created a virtual network. Both our DC-1 and Client-1 will exist on the same virtual network connectivity. There is nothing left for us to edit, so we will select “Review + Create”
  
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>We have now created the Domain Controller Virtual Machine. DC-1 will later be used as the central point for storing user accounts we create and managing/authenticating user access.</p>
<br />

  <h4>Creating our Client:</h4>
    <strong>What is the client?</strong>
    <p>The client, in relation to the domain controller, is a device that connects to the network domain and relies on the domain controller for user authentication, access permissions, etc. The domain controller is the 'big brain' the lets the device/client know what to do/who's allowed to do what. The client interacts with the domain controller to log in, access shared resources, etc.</p>














<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
