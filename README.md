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

[1]

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
