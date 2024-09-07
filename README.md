<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)
- PowerShell (Command-Line Interface)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Steps</h2>

- Step 1: Create Virtual Machines
- Step 2: Remote Desktop into VMs
- Step 3: Install & Run WireShark
- Step 4: Test Connectivity Between VMs
- Step 5: Alter Network Security Group Settings
- Step 6: SSH into VM
- Step 7: Observe DHCP Traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://github.com/user-attachments/assets/f56df175-621e-4708-a192-3ce25db68f03" height="80%" width="80%" alt="Disk Sanitiz!
ation Steps"/>
<img src="https://github.com/user-attachments/assets/b8a5d8c2-5885-406b-a709-46c4922da3c7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Step 1: Create Virtual Machines<h3>

 - Create two Virtual Machines in Azure
  - Search for "resource groups" 
  - Select "create" resource group 
  - Search for "Virtual Machines"  
  - Create two Virtual Machines with the following OS: Windows 10 OS for VM1 & Ubuntu OS for VM2
   (Make sure both VMs are in the same "resource group". I recommend using the same username and password for both VMs. The username and password will used to remote desktop into both VMs.)

<br />

<p>
<img src="https://github.com/user-attachments/assets/845e3300-ca12-4223-925d-3d2c570e8609" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/6ae0f46d-8e6e-445f-81e2-4e8b778eb10e" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/83c2a575-99ba-4368-8832-1bafc890c30d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/9aeb438f-723b-416a-8798-81a95f0e0334" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Step 2: Remote Desktop into VMs<h3>

- Remote Desktop into VM1. 
- Go to VM1 settings 
- Copy the "public IP address" 
- Search for "Remote Desktop Connection" in the start menu search bar 
- Open "Remote Desktop Connection" 
- Paste the copied IP address from VM1 and click connect 
- Use the username and password created in step 1 to sign in to the VM. 

<br />

<p>
<img src="https://github.com/user-attachments/assets/fd401356-3ed9-414e-81ab-829856f3d509" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/6f2f456d-aa32-459f-a75f-d958dac2dd5b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/a0e0d1e2-4346-45de-9d42-43979d09871b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/37c3bb06-4d14-490a-98b2-0ef1d17ad246" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Step 3: Install & Run WireShark<h3>

- On VM1, download WireShark. 
- Go to Wireshark official website [link](https://www.wireshark.org) 
- Download the Windows x64 (64-bit) installer
- Install WireShark 
- Open WireShark once installation is done
- Once Wireshark is open click "Ethernet" 
- Next, select the "shark symbol" in upper left hand corner to start monitoring network traffic 
- Use the filter bar to monitor specific network traffic (type "icmp" in filter bar to only see ping traffic). 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e0668ff5-ce23-47d8-aeab-46edbd3adaaf" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/f44020fd-3886-45d1-ae46-ef6628dcbcbf" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Step 4: Test Connectivity Between VMs<h3>

- Go to "PowerShell" on VM1  
- Type "ping -t" then paste VM2's private IP address into "PowerShell" to start a "perpetual ping" (WireShark will now show the two VMs pinging back and forth, allow the ping to continue). Pay attention to all of the data being communicated between both VMs on this step

<br />

<p>
<img src="https://github.com/user-attachments/assets/b412fbca-20aa-47a8-a45b-84ae199b36de" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/387ac7c4-740a-4c8b-b5c5-3783e56ea3b1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/729ac743-49b4-4de1-9b51-93137470582e" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/d46d8505-bade-4552-8935-281049fd153b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Step 5: Alter Network Security Group Settingss<h3>

- Go back to Azure Portal 
- Type "Network Security Group" in the search bar 
- Select "VM2-NSG" --> "Inbound security rules" under settings 
- Click "+ add" 
- In the new pop up window change "protocol" to "ICMP" 
- Change "action" to "deny" 
- Change "Name" to "Deny_ICMP_Ping" 
- Click "add" at bottom. 
*Notice how the ping cmd in PowerShell on VM1 is immediately halted due to being blocked by VM2's firewall thanks to the new inbound security rule you made. If you edit the rule to allow traffic, notice how the perpetual ping cmds successfully resume. *Press Ctrl+C to stop PowerShell ping
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/ff3df157-1108-4548-9d66-92f964b48a06" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/e04c0d2b-fd5a-4044-a5de-0ad308cac2b9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Step 6: SSH into VM<h3>

- SSH into VM2 from VM1 via PowerShell. 
- Type "SSH" into WireShark's filter bar 
- Go to "PowerShell" again, type "ssh labuser@(VM1 private IP address)" into the cmd line 
- Type "yes" to connection prompt 
- enter the password on next cmd line (password will not show but enter it anyway and press enter key, it will register) 
- You have now successfully remotely logged into VM2's command-line Interface (CLI). It should now read "labuser@VM2:". You can type a linux cmd such as: "id" to see the new network traffic between the VMs since linking via SSH. When finished exploring, type "exit" into cmd line on PowerShell to end the connection. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/a5193581-8615-40c1-ba90-4d114fa69a47" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Step 7: Observe DHCP Traffic<h3>

- Type "DHCP" into WireShark's filter bar 
- Open "PowerShell" , type "ipconfig /renew" to request a new IP address for VM1 from the Azure DHCP server (you may lose connection to the VM, if so, just RDH back into it) 
- You should see the newly issued IP address in WireShark. This concludes the lab.
</p>
<br />
