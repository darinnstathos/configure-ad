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

- Step 1: Set up Azure Resources: Domain Controller, Client-1 (Windows VM), setting Domain Controller's NIC Private IP to static
- Step 2: Ensure & Establish Connectivity between the Client & Domain Controller
- Step 3: Install Active Directory
- Step 4: Create an Admin account and normal user account in Active Directory
- Step 5: Join Client-1 (Windows VM) to the domain
- Step 6: Setup and Establish Remote Desktop for non-administrative users on Client-1
- Step 7: Create additional users and log into Client-1 with those users


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
    <p>We're creating a client device (Microsoft Windows) to witness the borrowing of user account information from our Domain Controller</P>
    
    1. Naviagte to Microsoft Azure and select 'Virtual Machines'
    2. Within the Virtual Machines creation portal, select the Resource Group we previously created: "AD-Lab" 
    3. We'll create the Virtual Machine Name: "Client-1"
    4. Select the image: Windows 10 Pro, version 22H2 -x64Gen
    5. Size: 2vcpus
    6. Create a username/password. In this example, I used my personal name again: darinstathos
    7. Under licensing, check the box: “I confirm I have eligible Windows 10/11 license with multi-tenant hosting rights
    8. Forward to the next page: Next: Disks >, Next: Networking >. Take note that Client-1 was automatically put on the same virtual network as our DC-1. This is important so that the two can later communicate with one another. 
    
    
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
    
<br />

<h4>Setting Domain Controller's NIC Private IP Address to Static:</h4>
<strong>Why is it necessary to change the Private IP Address to Static?</strong>
<p>Whenever we create resources such as Virtual Machines within Azure, there are several other resources created alongside it. One of these resources are NICs (Network Interface Cards) which have IP addresses automatically assigned to them via a DHCP server hidden within Azure. As mentioned previously, domain controllers provide centralized management and organization over user accounts, access permissions, and security policies within a network. 

 Given that it plays such a crucial role in centralizing and streamlining networking, we want our network devices to always be able to locate it for consistent instruction and communication. A static IP address prevents potential disruptions caused by a dynamic IP address. Thus, we must change the IP address from dynamic to static (meaning it uses the same IP address).</P>

