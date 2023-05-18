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

1. Navigate to Azure > 'Virtual Machines' > and select our Domain Controller "DC-1"
2. Select 'Networking' > Select 'Network Interface'
3. Select 'IP Configurations' > Select the current/only IPConfig there
4. Switch the assignment from 'Dynamic' to 'Static' and press 'Save'

<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

We have now switched the Domain Controller's NIC Private IP Address to be static. Now, there is more peace of mind in knowing that devices that connect to the domain and rely on the Domain Controller for user access/information will have secure connection and communication. 
<br>

  <h3>Step 2: Ensure & Establish Connectivity between the Client and Domain Controller</h3>
  
  <p>The next step is to ensure connectivity between the Client and the Domain Controller. We want to make sure that our machines can talk to one another so that once we add users accounts to the DC-1, Client-1 will be able to access them.</p>
  
  <h4>Log into Client-1 VM</h4>
  <p>First, we’re going to log into the Client-1 VM. To do this, we must retrieve the Public IP Address and connect it to Remote Desktop Connection (if you’re using WindowsOS) or Microsoft Remote Desktop (if you’re using MacOS).</p>
  
  1. In the Azure Portal, navigate to 'Virtual Machines'
  2. Select 'Client-1'
  3. Copy the Public IP Address to clipboard (Example shown: [X.X.X,X]
  4. Open Remote Desktop Connection (Windows) or Microsoft Remote Desktop (MacOS: can be downloaded from App Store)
  5. Paste the IP address
  6. Log in using the username/password created earlier in step 1. In this example, my username is my name: darinstathos

<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h4>Check Connectivity to Domain Controller (will fail at first attempt)</h4>
Now that we’ve logged into Client-1, we want to “ping” to our Domain Controller to make sure our machines can talk to one another/that there’s connectivity.

However, most likely, this “ping” will fail due to the Domain Controller’s firewall blocking ICMP traffic.

<strong>Why do firewalls sometimes block ICMP traffic?</strong>
Firewalls sometimes block ICMP (Internet Control Message Protocol) traffic as a security measure to protect against potential network vulnerabilities. ICMP traffic includes various types of messages, such as ping requests and error messages, which can be exploited for network scanning, denial-of-service attacks, or information disclosure. Blocking ICMP can help prevent these types of attacks and limit potential exposure of sensitive information. However, it's worth noting that ICMP is also used for legitimate network troubleshooting and diagnostic purposes, so firewall rules should be carefully configured to balance security and operational needs.

1. We need to get DC-1's private IP Address: Azure Portal > 'Virtual Machines' > DC-1
2. Copy the NIC Private IP Address. In this example, it's: 10.0.0.4
3. Navigate back to our Client-1 VM and open the command line in the search bar
4. Type: "ping -t 10.0.0.4" (-t means that it will ping continuously)
5. We can see the request failed/timed out because DC-1's firewall is preventing/blocking ICMP traffic from coming through

<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
