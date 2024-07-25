<p align="center">
<img src="https://cdn.worldvectorlogo.com/logos/microsoft-azure-2.svg" alt="osTicket logo"/>
</p>
<br>
<h1>Deploying Virtual Machines on Microsoft Azure</h1>
This tutorial offers a brief overview on how virtual machines are created using the Azure portal. Once the VMs are launched, Wireshark is then used to analyze network traffic traveling between both the virtual system and the host system.<br />


<h2>Video Demonstration</h2>

[![Virtual Machines in Microsoft Azure](https://markdown-videos-api.jorgenkh.no/url?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3Dx9WcHbfeW6U)](https://www.youtube.com/watch?v=x9WcHbfeW6U)


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (web portal)
- Remote Desktop Connection
- Wireshark

<h2>Operating Systems Used </h2>

- Windows 11</b> (22H2)
- Ubuntu Server (22.04)

<h2>Prerequisites</h2>

- An active membership with Microsoft Azure with at least $20 in credits
- ≥ 8 GB RAM (or expanded paging file)


<h2>TLDWatch Installation Steps</h2>

<p>
Before any virtual machines (VMs) can be created, you will first need to create a resource group in which your VMs will be housed. To do this, navigate to the "Resource Groups" icon from the <ins>All Services</ins> page. Alternatively, you can also type "Resource Groups" in the Azure search bar at the top of the UI.
</p>
<p>
<img src="https://github.com/user-attachments/assets/00786fa3-e3a9-492a-b781-1ed1adcf648c" height="80%" width="80%" alt="Resource Groups Button"/>
</p>
<br />

<p>
When creating your resource group, make sure you select the region that is closest to your host system. This isn't mandatory, but it *is* best practice for reducing latency while working inside your VMs.
</p>
<p>
<img src="https://github.com/user-attachments/assets/ae48b601-1087-42e0-b410-7404f7e4b4a4" height="80%" width="80%" alt="Geographic Location"/>
</p>
<br />

<p>
Once your resource group has been created, navigate to the "Virtual Machines" page in the same way you did for resource groups. Click "Create" --> 
"Azure Virtual Machine," and select the resource group you just created. Name your VM, and select the same region in which your resource group was created. Since the first VM will be a simple Windows 11 machine, select the image that gives you the least amount of system resources above the lowest option that only provides 1 vCPU and 1 GB of vRAM. For me, I chose the image which gave me 2 vCPUs and 16 GB of vRAM.

**Note: If Windows 11 does not show up in the drop-down menu, it may not be available in your region. In this case, you will need to change your region/zone.**
</p>
<p>
<img src="https://github.com/user-attachments/assets/8b51eb79-1522-44f4-8605-7d5b21340a10" height="80%" width="80%" alt="Windows VM"/>
</p>
<br />

<p>
After creating the username/password for the administrator account, navigate over to the <ins>Network</ins> tab and take note of the virtual network that is being created alongside your VM. This is the virtual network in which you will need to deploy your Ubuntu VM when we create it next.
</p>
<p>
<img src="https://github.com/user-attachments/assets/bac54e12-6a36-4438-bf96-b47b9f959c12" height="80%" width="80%" alt="VMs"/>
</p>
<br />

<p>
Once you create your Windows VM, you are going to need to wait about 5 minutes before you will be able to create your Ubuntu VM. The reason for this is because it takes some time for Microsoft Azure to fully establish the virtual network in which you will need to place your Ubuntu VM. It is much easier and faster to deploy a VM onto an already existing virtual network than it is to move it from one virtual network to another after its creation.

After waiting for a bit, create your Ubuntu VM in the same way you just created your Windows VM. Just be advised that you are limited to a total of 4 vCPUs during your initial trial of Microsoft Azure. Once both machines are deployed, return to the "Virtual Machines" page in the Azure portal, ensure they both have are connected with a public IP address, and copy the (public) IP address for you Windows VM.
</p>
<p>
<img src="https://github.com/user-attachments/assets/4ffc6817-3da8-4d17-962a-1858ca782f29" height ="80%" width="80%" alt="Deployment"/>
</p>
<br />

<p>
From your host system, navigate over to your OS's remote desktop application ("Remote Desktop Connection" if you're using Windows 11), paste the public IP address of the Windows VM into the computer/system field, enter the username/password you created for the Windows VM's administrator account, and log in. If done correctly, this should launch an instance of your Windows 11 VM on your desktop that you can minimize, maximize, move, etc.
</p>
<p>
<img src="https://github.com/user-attachments/assets/e954e542-11a6-4c53-87fb-49bc16129abb" height ="80%" width="80%" alt="VM Window"/>
</p>
<br />

<p>
Since your Windows 11 VM is in the same virtual network as your Ubuntu VM, you should be able to send and receive internet traffic across the two systems. To test this, you will, first, need the **private** IP address from your Ubuntu VM. This can be found by clicking on the name of your Ubuntu VM in the Azure portal and looking for "Private IP" in the <ins>Networking</ins> section.
</p>
<p>
<img src="https://github.com/user-attachments/assets/d1cb1e7f-ea75-48c2-bed7-7709bf6d9659" height ="80%" width="80%" alt="Private IP"/>
</p>
<br />

<p>
From your Windows VM, launch Windows Powershell and type the following:

**ping 0.0.0.0** (<-- replacing 0.0.0.0 with the private IP address of your Ubuntu VM)

You should then see ICMP request/ICMP reply traffic between both VMs. If you are unable to send/receive network traffic between your VMs, double check that you entered the correct IP address and that your VMs have been deployed in the same virtual network. You shouldn't have to make any changes to either of the system's firewalls since both Windows 11 and Ubuntu Server's firewalls allow outgoing ICMP traffic and incoming ICMP traffic respectively by default. 
</p>
<p>
<img src="https://github.com/user-attachments/assets/6f0b7f52-1876-4724-94db-34628c555160" height ="80%" width="80%" alt="Powershell"/>
</p>
<br />

<p>
To examine network traffic in further detail, launch Microsoft Edge (🤮) from your Windows VM, navigate to www.wireshark.org, and download the latest version of Wireshark. Once installed, launch the application, select "Ethernet," and click the blue fin in the upper-left corner. This should immediately start capturing the packets sent and received by your Windows 11 VM's virtual NIC. 
</p>
<p>
<img src="https://github.com/user-attachments/assets/e82bba12-6970-4080-a798-3fc4558ecd7b" height ="80%" width="80%" alt="Wireshark"/>
</p>
<br />

<p>
From here, you can enter different capture filters that will filter out network packets by ICMP, port number, or protocol. For example, if you wanted to examine pings sent and received across the network, you could enter,

**dhcp**

If you wanted to further refine your search to only include packets being sent from your host system to your Windows VM, you could use the filter

**tcp.port == 3389**

Should you decide to SSH into your Linux VM from the Powershell window, you could examine all session traffic between the two systems by entering

**ssh**
</p>
<p>
<img src="https://github.com/user-attachments/assets/c63845f2-a4b8-45f0-8908-9d630b0faf8b" height ="80%" width="80%" alt="Wireshark"/>
</p>
<br />

As you can see, there is quite a bit you can do with two VMs networked together via Microsoft Azure. By using Azure's virtualization technology, robust sandboxing environments can be created away from your host system allowing you to make, break, and experiment with systems of different operating systems, resources, and capabilities.

Have fun! 😊
