<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Inspecting Traffic Between Computers</h1>
In this project, we will use Wireshark, a protocol analyzer to observe network traffic between two virtual machines that we will create. If you haven't created a virtual machine before, check out my previous project here: https://github.com/brandenoz/virtual-machine. 

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Configuration Steps</h2>

<h3>Step 1: Setting Up Environment</h3>

I will create a resource group named “Network-Traffic”. When done with the project, I can delete the resource group to remove all resources used in this project rather than removing each resource. 
- First create a virtual machine named “Windows-VM”. 
- Choose Windows 10 Pro for the operating system, or image. 
- Choose at least 2 VCPUs for the size to make sure it is not too slow. 
- Make sure to click “I Confirm” on the box under Licensing.
- When you get to Networking, click "Create New" the Virtual Network. Name the Virtual Network "Virtual-Network-1". <br>

![1-Create a Virtual Network (1)](https://github.com/user-attachments/assets/b1995e37-9ab7-4a19-9255-ec9d616dce1d)

Create a Linux virtual machine. 
- Name the virtual machine "Linux-VM".
- Choose Ubuntu Server for the operationg system, or image.
- Choose at least 2 VCPUs for the size.
- Under "Administrator account" select Password for the Authentication type. Set your same username and password.
- Under Networking, make sure to select the same Virtual Network that we previously created, "Virtual-Network-1".
Make sure both of our virtual machines are on the same subnet. When you click on the two virtual machines, you should see that hte Virtual network/subnet is "Virtual-Network-1/default".

<h3>Step 2: Download Wireshark, a Protocol Analyzer</h3>

In our Windows 10 Virtual Machine we will Google Wireshark and download. 
- Go to wireshark.org and download the Windows x64 Installer.
- Click "Next" on everything through the installation process.
- Open Wireshark.
- Click on the blue Shark fin in the upper-left corner.

<h3>Filter by ICMP</h3>

In the Display Filter, which looks like a search bar, type "ICMP" and press Enter. <br>

![image](https://github.com/user-attachments/assets/7d039a35-e845-462b-93d4-2709896deaa1)

We are going to Ping to create some ICMP traffic. ICMP stands for Internet Control Message Protocol and Ping uses this protocol. 
- Get the Linux-VM's private IP address by clicking on the VM in the Azure Portal. The one I see is 10.0.0.5, but yours could be different if you are following along.
- Open Windows PowerShell, and type our Linux-VM's private IP address "ping 10.0.0.5" then press enter. <br>

![image](https://github.com/user-attachments/assets/563c830d-f0d8-4a8e-ac1f-ded7a8e49716)

- Now in Wireshark, when filtering for ICMP traffic, you will see the traffic from our ping we just executed. <br>

![image](https://github.com/user-attachments/assets/820fe288-69c1-4471-90c8-28f417cfdc77)

- In the ping from Windows Powershell you can see 4 replies from 10.0.0.5, but in Wireshark you can see 4 requests and 4 replies. This shows the requests from the Windows-VM and the replies from the Linux-VM.
- Going into the lower-left quadrant of Wireshark, we can analyze packets. If you click on the drop-down next to Ethernet II, you can see the MAC address of the source and destination of the packet. This will be MAC addresses of our Windows-VM (source) and Linux-VM (destination). MAC addresses are in layer 2 of the OSI model for networking. <br>

![image](https://github.com/user-attachments/assets/1b4f2398-a5c4-4cf5-90ad-b5a9d36cdf14)

- Now in PowerShell type "ipconfig /all" then press Enter. This will show the Windows IP Configuration which includes the MAC address. This should match what we already say in Wireshark. <br>

![image](https://github.com/user-attachments/assets/5853bd53-b2af-43fc-bb51-29709115d6e6)

- We will see that the MAC address is the same if we compare them. <br>

![image](https://github.com/user-attachments/assets/f2cc241d-2a75-4a87-ac57-3017ffbc107d)

- It is worth noting that a MAC address is typically fixed and stays the same, but in virtual machines, one is assigned.
- Under Internet Control Message Protocol if you click on Data, you can see that actual payload that is being sent in the ping protocol that the developers picked when designing it. In ping, the function is to establish connection rather than to send spedific data so the selection of the info is pretty random: "abcdefghijklmn opqrstuvwabcdefg hi". We are able to see this through the protocol analyzer but we would not know what the payload is when using ping on Windows PowerShell.<br>

![image](https://github.com/user-attachments/assets/b3f6734f-a595-4a4e-85ca-28b8db629735)

<h3>Step 3: Initiate a Perpetual Ping</h3>

In PowerShell type "poing 10.0.05 -t" then press Enter. 
- What we will notice is continual replies from 10.0.0.5 in PowerShell.
- In Wireshark we will see both the continual requests and replies of ICMP traffic from our ping.<br>

![image](https://github.com/user-attachments/assets/2d1b0555-378f-4b86-ae01-81c2cb967e65)

- In Wireshark we will click on the green recycle arrow in the shark fin to restart the capture, this will clear traffic and allow us to see the new incoming traffic better. We will notice that the traffic is continuous because our ping in PowerShell is still going. (until we tell it to stop or block ICMP traffic) <br>

![image](https://github.com/user-attachments/assets/b68a339b-4982-4134-a7c0-65c9942c337f)

<h3>Step 4: Block ICMP Traffic</h3>

In Azure, navigate to virtual machines and click on Linux-VM. From here click on Network Settings (under Networking) and click on Linux-VM-nsg under Network security group. <br>

![image](https://github.com/user-attachments/assets/2742fbc8-bcb9-4787-895e-302586205c9b)

- Click on Inbound security rules under Settings, then click Add. Type an "*" for Destination port ranges because ICMP doesn't use a port and * will mean any. Select ICMPv4 for the Protocal and select Deny for Action. Change the priority from 310 to 290 so that the rule gets evaulated first. Click Add.
- Once the rule takes effect, requests will begin to time out in our Windows-VM's PowerShell. <br>

![image](https://github.com/user-attachments/assets/08203905-f330-479f-89b6-cf62e2248edb)

- Requests only will also be seen in Wireshark: <br>

![image](https://github.com/user-attachments/assets/1c20f33f-0b14-47c2-8d79-46c56d635c94)

- We can now delete our security rule from the Linux-VM. When the the rule being deleted takes effect we will see replies in PowerShell from our Ping and we will see request and replies in WireShark for ICMP traffic. (remember you can restart the capture to see fresh traffic in Wireshark)
- Now we can stop our ping. Press Ctrl+C to stop the ping in PowerShell and stop the capture in Wireshark by clicking on the red square in the upper-left corner of the screen. 
