<h1>Active Directory Homelab</h1>


<h2>Description</h2>
This project demonstrates strong system administration skills, highlighting the ability to build and manage complex network environments. By configuring a Microsoft Server to host Active Directory and deploying a Domain Controller, it shows expertise in system architecture and domain management. Using PowerShell scripting to automate user creation further emphasizes proficiency in automation tools, improving administrative efficiency. Additionally, integrating Windows 10 Enterprise and Microsoft Server 2019 within a VMWare virtualized environment showcases adaptability and expertise in deploying modern technologies. Overall, the project reflects both technical proficiency and the ability to optimize complex IT systems, key skills for successful system administration.


<br />


<h2>Requirements:</h2>

- Oracle VM Virtualbox
- Microsoft Windows Server 2019
- Microsoft Windows 10 22H2


<h2>Objectives:</h2>

- Learn the basics of Active Directory and its role in managing networks
- Gain experience using virtualization tools (e.g., VMWare) to create and manage virtual machines
- Set up and manage a Domain Controller in a network
- Strengthen troubleshooting skills by resolving setup and configuration issues
- Use PowerShell scripting to automate Windows administrative tasks
- Understand domain connectivity and authentication, demonstrated by logging into user accounts within a domain

<h2>Overview of Lab </h2>

<a href='https://postimg.cc/V5pWxmHw' target='_blank'><img src='https://i.postimg.cc/bNNV2vxZ/Your-paragraph-text.jpg' border='0' alt='Your-paragraph-text'/></a>

<h2>Virutal Machine Configuration</h2>

- Install Windows Server 2019 on the virtual machine
- Use an internal network adapter for communication between virtual machines
- Password is necessary since this account will be an admin (Domain Controller)


<img src="https://i.postimg.cc/90S9M1Pt/Capture.png" width="425"/> <img src="https://i.postimg.cc/m2zMqfRd/Capture2.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/TYzmv73T/Capture4.png" width="425"/> <img src="https://i.postimg.cc/hGgdfXmw/Capture5.png" width="425"/> 


<h2>Virutal Machine Configuration</h2>

- Configure both network adapters: one for external access, one for internal networking
- Use a NAT adapter with the home router’s IP for external internet access
- Rename the adapters for clarity to help with Domain Controller (DC) and DHCP setup
- Set the internal network adapter IP to 172.16.0.1 (as shown in the diagram)
- Configure the DNS server IP as shown in the diagram, in preparation for Active Directory installation (which installs DNS automatically)
- Use a loopback address for DNS to allow the server to ping itself

<img src="https://i.postimg.cc/4dYVTrVN/Capture6.png" width="425"/> <img src="https://i.postimg.cc/RF2fCXL1/Capture7.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/WzrZdg5B/Capture8.png" width="425"/> <img src="https://i.postimg.cc/NGrmCvQd/Capture9.png" width="425"/> 



<h2>Installing Active Directory Services</h2>

- Install Active Directory Domain Services
- Create a domain-specific admin account (initially without admin privileges)
- intializing domain root name as mydomain.com
- Open Active Directory and grant the new account Administrator rights

<img src="https://i.postimg.cc/y8RG7bhj/Capture10.png" width="425"/> <img src="https://i.postimg.cc/GpJSyT5s/Capture11.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/c4g9cbzW/Capture12.png" width="425"/> <img src="https://i.postimg.cc/SKrDsFn6/Capture13.png" width="425"/> 


<h2>Configurating Remote Accesss and Routing</h2>

- Install and configure RAS/NAT on the Domain Controller
- Allows the Windows 10 client to access the internet through the internal network via the Domain Controller
- Ensure the NAT connection is set to the internet network adapter with the IP address 10.0.2.15

<img src="https://i.postimg.cc/Tw1q3yMx/Capture18.png" width="425"/> <img src="https://i.postimg.cc/5yvPfHL0/Capture19.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/y6XQpR1Q/Capture20.png" width="425"/> <img src="https://i.postimg.cc/J0fzBFsx/Capture21.png" width="425"/> 


<h2>Configurating DHCP Server</h2>

- DHCP automatically assigns IP addresses to devices on the network
- Create a scope that assigns IPs from 172.16.0.100 to 172.16.0.200 (100 available addresses)
- Set the lease duration to 8 days so assigned IPs stay reserved for each device during that period
- This prevents IP shortages and ensures new devices can connect to the internet


<img src="https://i.postimg.cc/gkbwjw3W/Capture14.png" width="425"/> <img src="https://i.postimg.cc/8z1jdqyX/Capture15.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/TP11Fnvr/Capture16.png" width="425"/> <img src="https://i.postimg.cc/rmWV2Jnt/Capture17.png" width="425"/> 


<h2>Creating Accounts For Domain</h2>

- Use a PowerShell script to create over 1,000 user accounts
- Script runs successfully with visual confirmation of the created accounts
- Some duplicates were skipped, but can be fixed by updating the script to handle them
- Example fix: add a "1" to the end of duplicate account names
- Full script is named CREATE_USERS.ps1 and is located at the top of the repository


<img src="https://i.postimg.cc/xCgs8r4r/Capture23.png" width="425"/> <img src="https://i.postimg.cc/RhFgnPkW/Capture24.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/d3x4R2F4/Capture25.png" width="425"/> <img src="https://i.postimg.cc/fW22hNdm/Capture26.png" width="425"/> 


<h2>Creating Client Virtual Machine</h2>

- Create a new Virtual Machine named CLIENT1 to act as a domain user
- Disable NAT on its network adapter to block direct internet access
- Require internet access only through the Domain Controller on the Server VM
- Set the network adapter to use the same internal network as the Domain Controller (per the initial diagram)
- Ensuring connectivity to domain and internet by pinging google.com

<img src="https://i.postimg.cc/d1MkznRz/Capture29.png" width="425"/> <img src="https://i.postimg.cc/fLxtZXBk/Capture30.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/VvmSN6Vg/Capture31.png" width="425"/> <img src="https://i.postimg.cc/VvgJ0wy8/Capture34.png" width="425"/> 


<h2>Verifying Active Directory Functionality</h2>

- Rename the computer to CLIENT1 and choose to join the domain mydomain.com
- Enter login credentials using the previously created Administrator account
- Log into a user account created by the PowerShell script to confirm setup
- Instead of using the VM’s default user, log into a domain user account with admin privileges
- Use Command Prompt to check if the client VM received an IP address from the Domain Controller
- Confirm successful IP lease from the Domain Controller
- Successfully ping the domain to verify proper network connectivity

<img src="https://i.postimg.cc/50nJw2tn/Capture35.png" width="425"/> <img src="https://i.postimg.cc/yxxHnCgB/Capture36.png" width="425"/> 


<br />


<img src="https://i.postimg.cc/6qPNmbnZ/Capture37.png" width="425"/> <img src="https://i.postimg.cc/gkwpZdbH/Capture38.png" width="425"/> 

<img src="https://i.postimg.cc/0Nv1FwMV/Capture39.png" width="425"/> 



