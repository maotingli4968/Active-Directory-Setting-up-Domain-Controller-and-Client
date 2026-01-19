<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Domain Controller in Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Installation Steps</h2>

<b>Create Resource Group and Virtual Network</b>
- In the Azure Portal, create a new Resource Group (e.g., <b>ActiveDirectoryLab</b>) in <b>East US 2</b>.
- Next, create a Virtual Network (e.g., <b>ActiveDirectoryVNet</b>) and assign it to the same Resource Group and region.
- This ensures that all subsequent VMs can connect to the same VNet.

<b>Create Domain Controller VM (DC1) </b>

- Go to <b>Virtual Machines</b> → <b>Create</b>.
- Resource Group: <b>ActiveDirectoryLab</b>
- VM Name: <b>DC1</b>
- Region: <b>East US 2</b> (same as the VNet)
- Image: <b>Windows Server 2022 (Hotpatch)</b>
- Size: <b>at least 2 vCPUs</b>
- Licensing: check the required box
- Networking: attach to <b>ActiveDirectoryVNet</b> with the default subnet
- Click <b>Review + Create</b> → <b>Create</b>

<img width="975" height="896" alt="image" src="https://github.com/user-attachments/assets/aaec4610-049d-4db8-81bb-201298293ec4" />


<b> Create Client VM (windows-vm)</b>

- You can create a new VM or keep using the old VM created in previous tutorials.


<b>Configure Static Private IP for DC1</b>

- By default, Azure assigns a dynamic private IP to VMs. For a domain controller, we need a static private IP so the address never changes.
- In the Azure Portal, open <b>DC1</b> → <b>Networking</b> → <b>NIC</b> → <b>IP configurations</b>.
- Change <b>Private IP assignment</b> from <b>Dynamic</b> to <b>Static</b>.
- Save changes.
- This ensures <b>Client1</b> can always resolve <b>DC1</b> when using it as the DNS server.

  <img width="975" height="660" alt="image" src="https://github.com/user-attachments/assets/f1480537-8aef-4c30-ac0d-5dfc16ad0417" />

  <b> Disable Firewall on DC1 (Lab Purposes Only)</b>

- Log into <b>DC1</b> using Remote Desktop with its public IP.
- Open <b>Run</b> → <b>wf.msc</b>.
- In Windows Firewall settings, disable the firewall for <b>Domain</b>, <b>Private</b>, and <b>Public</b> profiles.
- This is only for the lab to prevent pings from being blocked.

  <img width="883" height="469" alt="image" src="https://github.com/user-attachments/assets/84efe7b2-edbb-4f3e-83e6-3c0a3bf7abe8" />


  
<b>Configure Client1 DNS to Point to DC1</b>

- By default, <b>Client1</b> uses Azure DNS. To join the domain, it must use <b>DC1</b> as DNS.
- In the Azure Portal, open <b>Client1</b> → <b>Networking</b> → <b>NIC</b> → <b>DNS servers</b>.
- Change setting from <b>Inherit from virtual network</b> to <b>Custom</b>.
- Enter <b>DC1’s private IP address</b>.
- Save changes.
- Restart <b>Client1</b> VM to apply settings.

<img width="975" height="768" alt="image" src="https://github.com/user-attachments/assets/cc3a50f9-878b-4578-8d07-23007dce5127" />


## Test Connectivity from Client1 to DC1

- Log into <b>Client1</b> using Remote Desktop with its public IP.
- Open <b>PowerShell</b>.
- Run:

```powershell
ping <10.0.0.6>
```
- You should see successful replies. If you see <b>Request timed out</b>, check that:
  - Both VMs are on the same <b>VNet</b>.
  - <b>DC1</b> firewall is disabled.

<img width="975" height="763" alt="image" src="https://github.com/user-attachments/assets/54851b2d-0782-49cc-8b44-7d34432a2847" />












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

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
