# Setting up a Cybersecurity Homelab for Intrusion Detection and Monitoring  
## _**What you should expect**_

This walkthrough will equip you with practical skills to build a cybersecurity homelab. Building a Cybersecurity home lab utilizing VMware and pfSense for secure network simulations, apply Security Onion for threat detection, and prepare Kali Linux for attack simulations. You'll also gain experience in configuring Windows systems and using Splunk, as well as integrating various Linux machines for diverse cybersecurity scenarios. I will be following along with cyberwox and cybermentor's walkthroughs for this homelab. We also used domain controllers and Active Directory.

![image](https://github.com/user-attachments/assets/43913107-9732-46a0-a08f-d0bb7c4351c4)   


 Here I will be creating a small scale enterprise environment for testing tabletop exercises and configure essential cybersecurity tools locally. Here is a quick run down of our entire configuration.

## _**Content**_

*   _Requirements for Host Desktop_
    
*   _Setting up VMware Workstation as Hypervisor_
    
*   _Configuring pfSense Firewall for Enhanced Network Segmentation and Security_
    
*   _Setting up Security Onion as a Comprehensive IDS, Security Monitoring, and Log Management Solution_
    
*   _Preparing Kali Linux for Attack Simulations_
    
*   _Setting up a Windows Server to Function as a Domain Controller_
    
*   _Configuring Windows-Based Desktops_
    
*   _Configuring Splunk on Ubuntu Server_
    
*   _Installing Splunk's Universal Forwarder_
    

Below is my personal build that i'll be using for demonstrational purposes


*   _MSI B450 Motherboard_
    
*   _MSI Nvidia GeForce RTX™ 4060_
    
*   _Ryzen 5 2600 6-Core 3.4GHz_
    
*   _G.Skill 4000 MHz 16GB RAM_
    
*   _Windows 11 Pro 23H2_

*   _SSD 1TB/HDD 2TB_

## **Requirements for Host Desktop**

For this module, you'll need a computer with a _**minimum of 16GB of RAM**_ to effectively run multiple virtual machines simultaneously. Sufficient RAM and Decent CPU will suffice. Also, ensure you have ample storage space for the software installations and data generated during the lab activities. Having a reliable internet connection is beneficial for accessing online resources and updates. Prior knowledge of basic network and system operations is recommended, but not mandatory. Please note that the specifications may vary depending on the complexity of the scenarios you wish to simulate in your homelab.

It's important to use a separate setup for your lab, one that doesn't hold any critical data like personal files or sensitive information. This setup will serve as your experimental playground. But I will be doing it on my main host computer if you're wondering.

## **Setting up VMware Workstation as Hypervisor**

Virtual machines can be thought of as individual computers housed within your own computer. They are indispensable in the realm of cybersecurity, providing a safe space for testing and experimentation, thereby shielding your main system from potential harm.

For this walkthrough, we will primarily be using VMware on Windows.

**Download VMware:** Get the Free VMware Workstation from their website

1.  Navigate to the [VMware Download Center](https://customerconnect.vmware.com/web/vmware/downloads).
    
2.  Locate **VMware Workstation Player** under Desktop & End user Computing.
    
3.  Select the installer from the list according to your host operating system.
    
4.  Click **Download**.
    

**Run Installer:** > **Follow Steps:** > **Agree to License:**  > **Select Typical Setup:**  > **Choose Preferences:**  > **Install:**  > **Open VMware:** 

## **Configuring pfSense Firewall for Network Segmentation and Security**

In our cybersecurity homelab, we rely on pfSense, an open-source firewall and router software. Its first crucial function is network segmentation, which separates our virtual machines into different network zones, mirroring a realistic network scenario. Secondly, pfSense enhances our lab's security by inspecting network traffic and blocking any potential threats, safeguarding our learning environment. Furthermore, the hands-on experience we acquire in managing network security through pfSense is invaluable for any cybersecurity practitioner. Overall, in this lab, pfSense will be an integral tool, assisting in effectively managing and securing our virtual network.
   
**Download pfSense:** Since pfSense doesn't offer ISO files directly anymore, I download the iso file from Dakota State University's repo.
   
![image](https://github.com/user-attachments/assets/76a3062c-98fc-4193-878e-195c692ddf22)
       
1.  Choose your architecture, and find the latest mirror .
    
    ![image](https://github.com/user-attachments/assets/9fb0ef79-60b5-4356-a782-35ebe9f3a08d)


**Install pfSense on a Virtual Machine:** Create a new virtual machine in your hypervisor (VMware, VirtualBox, etc.), and install pfSense.

1.  Launch VMware Workstation Player
    
2.  Select **Create a New Virtual Machine**
    
    ![image](https://github.com/user-attachments/assets/5873a093-2b85-4fcc-a0d3-386d3af717f5)

    
3.  Select the path to your pfSense ISO image by clicking **Browse**, then click **Next**.
    
   ![image](https://github.com/user-attachments/assets/ed85bde2-df27-4ca2-84b1-87dc6893957a)
   
4.  Rename Virtual Machine Name to pfSense, then click **Next**.
    
5.  Leave Maximum Disc Size at 20GB, and Split Virtual Disks into multiple files. Click **Next**.
    
6.  Select 2GB of RAM
    
    ![image](https://github.com/user-attachments/assets/3bdcd175-a207-4944-93fe-26388c24ee87)

   
7.  Click on **Add** to add 5 more Network Adapters to support the other Virtual Machines for this Lab. Then, under Network Connection, click **Custom** and assign each specific virtual network with each of the new Network Adapters in the dropdown to their respective networks. Once finished, click **Close**.
    
    ![image](https://github.com/user-attachments/assets/a59cde01-ca85-48b1-bf49-50f2266582c9)
   
8.  Click **Finish**.
    
9.  Follow the instructions and hit enter for all default settings. When you get to **ZFS Configuration** hit the Space Bar to choose the partition and the **\*** will appear. Hit enter to proceed.
    
    ![](https://framerusercontent.com/images/l9SMao3oMGwT7zhmCrSvOwISgk.png)    
10.  Hit enter to proceed.
    
   ![](https://framerusercontent.com/images/flaX6l0jkGIDI16pejCaZp6DcM.png)    
11.  After the installation is finished, click on **Reboot.**
    

**Set Up WAN and LAN:** During the installation, set up your WAN (Wide Area Network) and LAN (Local Area Network) interfaces according to your network architecture.

1.  Enter **1** to assign interfaces. Enter **n** for VLAN.
    
    ![](https://framerusercontent.com/images/BRVF6DRnHNbNhiRxcMpGrGXWUw.png)    
2.  Enter **em0, em1, em2, em3, em4, em5** respectively to assign each network. Click **y** when finished.
    
    ![](https://framerusercontent.com/images/QskwjR13miSCjaZa2LDNDldZA.png)    

**Access Web Interface:** Once installed, access the pfSense web interface via the IP address assigned to the LAN interface.

1.  Enter **2** to assign IP addresses. Enter **2** to configure the LAN interface. Enter **n** for DHCP.
    
    ![](https://framerusercontent.com/images/5Eszw0Q7N65kY3yxe68LEUIB9bo.png)    
2.  Enter your **LAN IPv4**. Then enter **24** for subnet.
    
    ![](https://framerusercontent.com/images/RGbzgXmTr2GSYyUjIKJK1uDWWA.png)    
3.  Enter **Space Bar** for none. Enter **n** for DHCP6. Enter **Space Bar** for none. Enter **y** for DHCP server on LAN. Enter **192.168.1.11** for the IPV4 start address range. Enter **192.168.1.200** for the IPV4 end address range. Enter **n** for HTTP configuration. Press **Enter** to continue.
    
    ![](https://framerusercontent.com/images/N6MszkVSE8u1Y96NNhvLzdKNTE.png)    
4.  Enter **2** to assign option 1 interface. Enter **3** for OPT1 interface. Enter **n** for DHCP. Enter **192.168.2.1** for OPT1 IPv4 address. Enter **24** for subnet. Enter none for LAN gateway address. Enter **n** for DHCP6. Enter none for IPv6 address. Enter **n** for DHCP server. Enter **n** for HTTP.
    
    ![](https://framerusercontent.com/images/IKQBaaNXBcFk2OUxQ2nh83dyM.png)    
5.  Repeat step 4 for the remainder of OPT2 (**192.168.3.1)**, OPT4 (**192.168.4.1)**. It should look like this:
    
    ![](https://framerusercontent.com/images/XW8lh8MhLFe1zKLkle3YknRUst4.png)    

## **Setting up Security Onion as a Comprehensive IDS, Security Monitoring, and Log Management Solution**

In our cybersecurity homelab, we implement Security Onion, a powerful open-source software designed for **network security monitoring, intrusion detection, and log management**. An integral component of any robust cybersecurity setup, Security Onion serves as our comprehensive Intrusion Detection System (IDS). This means it constantly monitors our network traffic, raising alerts when it detects potential threats. But it goes beyond simply detecting threats. It provides us with sophisticated tools that enable in-depth investigation into these potential security breaches, offering invaluable insights to mitigate future risks. Additionally, Security Onion assists us in efficiently managing system logs - a critical part of incident response, as these logs record the events happening on our virtual machines. By integrating Security Onion into our lab, we not only enhance our ability to detect and investigate potential threats but also gain a practical understanding of how professional cybersecurity systems function.

**Download Security Onion:** Visit the Security Onion website and download the latest version.

1.  [Download the Security Onion ISO file here](https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md#download-and-verify)
    

**Create a Virtual Machine:** Use your hypervisor (like VMware) to make a new virtual machine. Then, install Security Onion on it.

1.  Launch VMware.
    
2.  **Create a New Virtual Machine** and find the path to your **Security Onion ISO image file**.   
    ![image](https://github.com/user-attachments/assets/0894fdb6-08cf-4b03-9a95-0b2f3e23ccea)   

3.  Select **Linux** and **CentOS 7 64-bit**. Click **Next**.
    
    ![image](https://github.com/user-attachments/assets/ed514b03-d1c3-4f9f-a6c3-27e161d71cc2)
 
4.  Choose a name for your Virtual Machine. We will use **SecurityOnion**. Click **Next**.
    
5.  I am setting Disk Size to **200 GB** and Store virtual disk in a **single file**. Click **Next**.
    
    ![image](https://github.com/user-attachments/assets/c04ad417-ad7e-43b4-8f19-3ff355e9375a)
  
6.  Click **Customize Hardware**.
    
7.  I am Setting my memory to **6.5 GB** of RAM.
    
8.  Set number of processor cores to **4**.
    
    ![image](https://github.com/user-attachments/assets/f504d34d-383e-45f6-b648-57849542d93e)   
    
9.  Click **Add** to add two network adapters and set **Network Adapter 2 to VMnet4** and **Network Adapter 3 to VMnet5**. Click **Close**.
    
    ![image](https://github.com/user-attachments/assets/e6ee9400-b464-40cb-829f-467f872ffec1)   

10.  Click **Finish**.
    
11.  **Launch** SecurityOnion Virtual machine.
    

**Run Setup:** Open Security Onion and follow the setup wizard. You'll set your network settings and choose which services you want to run.

1.  Type **yes** and hit **Enter**.
    
    ![](https://framerusercontent.com/images/lbPPtF74MkVyOBqPMqypexK0jBs.png)    
2.  **Create** a username and password. Hit **Enter**.
    
    ![image](https://github.com/user-attachments/assets/caef5b89-7647-429c-971c-9ea767e132bc)   
 
3.  Once it's finished, hit **Enter** to reboot.
    
4.  **Login** with credentials. Hit **Enter**.
    
    ![image](https://github.com/user-attachments/assets/e333b6ce-e76a-4cd1-ad3b-76c23b8e6213)   
    
5.  Hit **Enter** to select yes to continue installation.
    
6.  Hit **Enter** to run the standard installation.
    
    ![](https://framerusercontent.com/images/kYCdnunCPDXJBQ6Na97QrcpNnw.png)    
7.  Hit **Enter** for evaluation.
    
8.  Type **AGREE**. Hit **Enter**.
    
9.  Hit **Enter** for hostname.
    
    ![image](https://github.com/user-attachments/assets/81a1b146-7bc5-49bc-b080-fc13665f0f87)
   
10.  Select **Use Anyway**.
    
11.  Press **Space Bar** to select the first interface.
    
     [](https://framerusercontent.com/images/xCAiTal0RfEW2s0ObESfXDas5V0.png)    
12.  Scroll down and select **DHCP** and press **Space Bar**. Hit **Enter**.
    
   ![](https://framerusercontent.com/images/ESe0l8hsSGf4nZ40TkFAJxIIqk.png)    
13.  Hit **Enter** to confirm.
    
14.  Hit **Enter** to select ok.
    
15.  Hit **Enter** for standard internet access.
    
  ![](https://framerusercontent.com/images/nkD7xHGTu3WjUswiw6wz44ehI.png)    
16.  Hit **Enter** for Direct.
    
   ![](https://framerusercontent.com/images/5FU74Ute7XxRllk5Za2nHxBwi0.png)    
17.  Select **ens35** and press **Space Bar** for monitor interface. Hit **Enter**.
    
  ![](https://framerusercontent.com/images/LWuWOmGKXnp6B7Vx0wx3YoQ.png)       
18.  Hit Enter for Automatic updates.
    
19.  Hit Enter for default home network.
    
20.  Choose your optional services and hit Enter.
    
21.  Hit Enter to select default docker IP range.
    
22.  Enter an Email Address and Password.
    
  ![image](https://github.com/user-attachments/assets/47877950-3b5b-4cf5-ba2f-5ce5c43f2cec)   
       
23.  Use IP address. Hit Enter.
    
   ![](https://framerusercontent.com/images/xkF6lpIymswIz7TcrHva4Vy2hQ.png)    
24.  Hit Enter to configure ntp servers.
    
25.  Hit Enter for default ntp servers.
    
26.  Select **No**. We will configure later.
    
  ![](https://framerusercontent.com/images/iDMybSo0EbsncHyPXUYTzAdKNzo.png)    
27.  Hit **Tab** to select yes and **Enter** for yes. (**Take note of Access URL** for later. Yours will be different).
    
  ![image](https://github.com/user-attachments/assets/6502deff-e188-4b6d-8051-b608814233e0)   
   
28.  Once finished, hit **Enter** to reboot.
    

**Configure so-allow**: We will first install Ubuntu on VMware. Once installed continue with this section. We will be in Ubuntu then back to Security Onion.

**Installing Ubuntu on VMware**:

1.  [Download Ubunto here.](https://ubuntu.com/download/desktop)
    
2.  Launch VMware.
    
3.  Create a New Virtual Machine
    
4.  Select the path for your Ubuntu ISO image.
    
5.  Assign a name to your virtual machine and a username and password.
    
    ![image](https://github.com/user-attachments/assets/53e64b6c-0885-40fd-81bf-76c6be8f2fdd)   
  
6.  Click next on all the default settings and click finish.
    
7.  Launch Ubuntu and choose your settings for installation.
    
    ![image](https://github.com/user-attachments/assets/effed98e-2737-4d7e-a33b-aa069cf5f37d)   
  
8.  Once finished, select **restart**.
    
9.  Login with your credentials.
    
10.  Open terminal in Ubuntu.
    
11.  Type ipconfig
    
12.  Type sudo apt install net tools
    
13.  Type your password for sudo
    
   ![image](https://github.com/user-attachments/assets/8ba2e6b2-bf27-495a-a683-cbc1295cb32b)   
  
14.  Type ifconfig. Note down the IP address in the image.
    
  ![image](https://github.com/user-attachments/assets/051e8940-c042-4451-9bc4-a51c766936c1)   
   
15.  Launch Security Onion
    
16.  Login with your credentials.
    
17.  Type in sudo so-allow. Type in your sudo password.
    
   ![image](https://github.com/user-attachments/assets/d2fe609b-8717-4e8c-bdc1-54a5d0127d96)   
   
18.  Type a to select Analyst.
    
19.  Type in the IP address you noted down from ifconfig in Ubuntu. Hit Enter.
    
  ![image](https://github.com/user-attachments/assets/034ae995-1ec3-4e5d-9a95-dd722a626cd1)   
   
20.  Go back to Ubuntu.
    
21.  Launch Firefox.
    
22.  Type in the security onion web interface from the image above it's your splunk IP In my case it's 192.168.230.134.
    
  ![image](https://github.com/user-attachments/assets/9403dea0-24ea-401c-a532-a2d819665029)   
    
23.  Click Advanced.
    
24.  Click Accept the Risks and Continue.
    
25.  Log in to Security Onion.
    
   ![image](https://github.com/user-attachments/assets/389e24da-4d9b-49aa-b7d8-e9fb85574415)   
   

**Test Setup:** Finally, make sure everything works, as you can see here we're already getting some alerts

![image](https://github.com/user-attachments/assets/5aa5bd4e-61ef-4da4-83f9-b3207f4fea1d)   
  

## **Preparing Kali Linux for Attack Simulations**

Kali Linux is a specialized Linux distribution, specifically tailored for digital forensics and penetration testing. It's a fundamental tool in our cybersecurity lab due to its capacity for simulating various types of cyber attacks. Loaded with numerous security tools, Kali Linux enables us to emulate real-world attacks on our virtual network. This process is not only eye-opening, revealing how such attacks transpire and how to thwart them, but also equips us with the practical experience of using professional cybersecurity tools. Most importantly, all of this is conducted within the confines of our controlled lab environment, eliminating any risk to actual systems or breaching legal boundaries. In short, by preparing Kali Linux for attack simulations in this lab, we are paving a crucial path for hands-on learning in cybersecurity.

**Download Kali Linux:** Visit the Kali Linux website and download the latest version.

1.  [Download Kali Linux here](https://www.kali.org/get-kali/#kali-virtual-machines). (We can download the Pre-built Virtual Machine)
    

**Create a Virtual Machine:** In your hypervisor (like VMware), Open a new virtual machine. Install Kali Linux on this virtual machine.

1.  Launch VMware.
    
2.  Open a New Virtual Machine.
    
3.  Select the path to Kali Linux VM image.
    
4.  Select Edit virtual machine settings.
    
    ![image](https://github.com/user-attachments/assets/786b0929-d504-43c2-8e0c-52602e8c5337)   
  
5.  Set your Maximum disk size. We will choose 4GB.
    
6.  Click Add to add a network adapter. Map it to VMNnet2. Click OK.
    
7.  Click Finish.
    
8.  Launch Kali Linux.
    
9.  Login credentials are:  
    **Username: kali**
    
    **Password: kali**
    
10.  Open Terminal type in passwd and change the default password to a more secure one of your choice.
    
  ![image](https://github.com/user-attachments/assets/d4a328b5-db1e-462f-b7ba-d3b93663df12)   
   

**Access pfSense WebConfigurator to make changes to the firewall rules:**

1.  Open Firefox (or another web browser) and enter IP address https://192.168.1.1 in the URL. Go to Advanced and select Accept the Risk and Continue.   
(At this part of the lab I had trouble accessing the IP address so I realized the Ip was unreachable after some testing, I came to the conclusion that Kali needed an IP assigned so I ran the following command)

    ![image](https://github.com/user-attachments/assets/44b8e498-be86-47e2-8e26-ebcd76f26b30)   

    
    ![image](https://github.com/user-attachments/assets/9ba8fc56-62d1-49d2-8353-f4d777adcbcb)
    
2.  Login with
    
    Username: admin
    
    Password: pfsense
    
3.  Click Next until you reach General Information.
    
4.  Primary DNS Server is: 8.8.8.8
    
    Secondary DNS Server is: 4.4.4.4
    
    ![](https://framerusercontent.com/images/AR8EKAb544HJRx5AYRWeTZr3M.png)    
5.  Select your Timezone.
    
6.  Click Next.
    
7.  Click Next for LAN interface.
    
8.  Create a new password.
    
9.  Select Reload.
    
10.  Click Finish.
    
  ![](https://framerusercontent.com/images/8WUXFDsB5oCMqo3lDYX0V4R4Nw.png)    

1.  Navigate to interface > LAN.
    
2.  Type Kali in Description
    
    ![](https://framerusercontent.com/images/DGTPc7Tlh0iMQyStQtwbiLMpU.png)    
   
3.  Navigate to Interface > Kali.
    
4.  Select None for IPv6 Configuration Type.
    
5.  Click Save and Apply Changes.
    
6.  Navigate to Interface > OPT1.
    
7.  Rename it to VictimNetwork.
    
8.  Click Save and Apply Changes.
    
9.  Navigate to Interface > OPT2.
    
10.  Rename it to SecurityOnion.
    
11.  Click Save and Apply Changes.
    
12.  Navigate to Interface > OPT3.
    
13.  Rename it to SpanPort.
    
14.  Check the box for **Enable** to enable interface.
    
15.  Click Save and Apply Changes.
    
16.  Navigate to Interface > OPT4.
    
17.  Rename it to Splunk.
    
18.  Click Save and Apply Changes.
    
   ![image](https://github.com/user-attachments/assets/4e34ec3f-a48d-4a23-8c66-b790bd405f44)   
  
19.  Navigate to Bridges.
    
20.  Click Add.
    
21.  Choose VictimNetwork for Member interfaces.
    
22.  Click Display Advanced.
    
23.  Choose SpanPort for Span Port.
    
34.  Click Save.
    
  ![image](https://github.com/user-attachments/assets/80f92406-eb0d-42bf-a88d-e507e9fc1bd3)   
 
31.  Navigate to Firewall > Rules.
    
32.  Click Add.
    
33.  Change Protocol to Any. Click Save.
    
   ![image](https://github.com/user-attachments/assets/b95dc22f-e41a-4aad-939b-ea11584ab14f)   
   ![image](https://github.com/user-attachments/assets/6f19b1b1-1365-4740-b313-86c47a947d71)   

  

## **Setting up a Windows Server to Function as a Domain Controller**

A Domain Controller, think of it as the boss of a Windows network, is in charge of all the important stuff like user logins, deciding who gets to access what, and making sure resources are shared correctly. It uses a nifty tool known as Active Directory, which is like the 'phonebook' of the network, keeping a record of all users and computers.

Having a Windows Server as a Domain Controller in our cybersecurity homelab is super valuable. It mirrors what we'd see in a real business setting, letting us learn about managing a network. What's even better is that our lab is a secure environment, so we can experiment and play around with different settings, exploring the ins and outs of cybersecurity without worrying about messing up a real network.

Download Windows Server 2019 and Windows 10:

1.  [Download Windows server 2019 Evaluation Copy here.](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)
    

**Create a Virtual Machine:** First, use your hypervisor (like VMware) to create a new virtual machine. Install Windows Server on this machine.

1.  Creates a New Virtual Machine.
    
2.  Select the Path to your Windows Server 2019 ISO.
    
3.  Proceed without Product Key.
    
4.  Name your Virtual Machine.
    
5.  Select your storage. We will use 60GB.
    
6.  Select Customize Hardware.
    
7.  Set default Network Adapter to custom: VMnet3.
    
8.  Remove Floppy Drive.
    
9.  Click Finish.
    
    ![image](https://github.com/user-attachments/assets/4264611e-d594-4446-b2ca-381431e8c931)   
  
  
10.  Launch Windows Server 2019.
    
11.  Make sure to press any button to continue.
    

Windows Setup:

1.  Click Next, then Install Now.
    
    ![](https://framerusercontent.com/images/OH0fqYv0DSPvt7280iz8OZTXr4.png)    
2.  Select the second option for Desktop Experience.
    
    ![](https://framerusercontent.com/images/nQwaw2i17mJdm5Lwu2WUyDvYzmc.png)    
3.  Accept the license and terms.
    
4.  Choose Custom Install.
    
5.  Select New, Apply, Then OK.
    
    ![](https://framerusercontent.com/images/bmVfd954cV8mVXfH7Q2v2hHNno.png)    
6.  Create a new password.
    
    ![](https://framerusercontent.com/images/ejcHdiyrCmpynXL3I9n48Xwp7E.png)    
7.  Login with your credentials and launch Server Manager Dashboard.
    

**Configure Server:** Start the Server Manager Dashboard, select "Add roles and features," and then click "Next" until you get to "Server Roles."

1.  First, lets rename your pc by navigating to settings > Search for "About this PC" > Rename this PC.
    
    ![image](https://github.com/user-attachments/assets/65e1ea50-38a6-49e6-8e30-ac4bb98a92c8)   
  
   
2.  Restart your Virtual Machine.
    
3.  Go to Server Manager Dashboard.
    
4.  Navigate to Manage > Add Roles and Features.
    
5.  Click Next.
    
6.  Click Next for Role-based or Feature-based installation.
    
7.  Click Next on Server Selection.
    
8.  Select Active Directory Domain Services and click Add Features. Click Next.
    
    ![image](https://github.com/user-attachments/assets/8ec5ae44-ae1a-4187-818f-f7ebe2ac7a56)   
   
9.  Click Next for Features.
    
10.  Click Next for AD DS.
    
11.  Click Install for confirmation.
    
12.  After installation is finished, navigate to notifications > promote this server to a domain controller.
    
  ![image](https://github.com/user-attachments/assets/ca8953a5-90cf-4e88-aa1f-67f41d35e57c)   
  
13.  Select Add a new forest. Enter a Root domain name ending in **.local**. Click Next.
    
  ![image](https://github.com/user-attachments/assets/5f8914cc-3196-4d5b-a1fc-59c1589969bf)   
    
14.  Setup your password. Click Next.
    
15.  Click Next for DNS options.
    
16.  Click Next once verified.
    
   ![image](https://github.com/user-attachments/assets/ec29721f-454d-4ebd-a69d-6c4a59db2052)   
    
17.  Click Next for Paths.
    
18.  Click Next for Review Options.
    
19.  Click Install. The system will be rebooted when finished.
    
   ![image](https://github.com/user-attachments/assets/4241d1ca-5ea9-4ab8-94e3-bdd882ec0d5f)   
    

**Install AD DS:** In "Server Roles," select "Active Directory Domain Services" (AD DS). This will prompt you to add some additional features. Click "Add Features," then "Next," and finally "Install."

1.  Go to Server Manager Dashboard.
    
2.  Navigate to Manage > Add Roles and Features.
    
3.  Click Next on before you begin.
    
4.  Click Next on installation type.
    
5.  Click Next on Server Selection.
    
6.  Select Active Directory Certificate Services and Add Features. Click Next.
    
7.  Click Next on Features.
    
8.  Click Next on AD CS.
    
9.  Check Restart the destination server automatically if required. Then click Install.
    
    ![image](https://github.com/user-attachments/assets/f34556c4-3b37-4ede-ace4-620bd9016aa3)
   
    
10.  Once finished, Navigate to notifications > Configure active directory certificate services on the destination server.
    
11.  Click Next on Credentials.
    
12.  Select Certification Authority. Click Next.
    
13.  Select Validity Period on the left pane and set to 99 years. Select Next.
    
14.  Select Next on Certificate Databse.
    
15.  Select Configure on Confirmation.
    
16.  Click Close.
    
17.  Manually reboot.
    

Add Users:

1.  Navigate to tools > Active Directory Users and Computers.
    
2.  Select your domain > Users > Rick click > New > User.
    
    ![image](https://github.com/user-attachments/assets/632c7ecb-4c23-46e4-8cf9-225eb703a320)   
    
3.  Create your user. Click Next.
    
4.  Create your password. Finish.
    
    ![image](https://github.com/user-attachments/assets/ac68940c-f27e-4402-b540-c3cd330b91c0)
   

**Log In:** After the restart, you can log in with your domain administrator credentials.

**Test Setup:** Open "Active Directory Users and Computers" to see if everything is working correctly.

**Optional Disable Firewall:** Disable firewall for learning purposes for this lab environment.

1.  Search for windows defender firewall.
    
2.  Turn off windows defender firewall on the left pane. Turn off all options.
    

**Add pfSense as default gateway:**

1.  Navigate to Control Panel
    
2.  Navigate to Network and Internet > Network Connections > Your network.
    
    ![image](https://github.com/user-attachments/assets/0b60d97e-6fb0-42dd-9980-5d9b4c7eab71)
    
3.  Right click > Properties.
    
4.  Select Internet Protocol Version 4 (TCP/IPv4)
    
5.  Select Use the following IP address.  
    IP address: 192.168.2.10
    
    Subnet Mask: 255.255.255.0  
    Default gateway: 192.168.2.1
    
6.  Select Use the following DNS server address:  
    Preferred DNS server: 192.168.2.1
    
7.  Click OK.
    

## **Configuring Windows-Based Desktop**

1.  [Download Windows 10 Evaluation Copy here.](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise)
    
2.  Launch VMware.
    
3.  Create a New Virtual Machine.
    
4.  Select the path to your Windows 10 ISO.
    
5.  Skip the product key and click Next.
    
6.  Name the virtual machine the first user you set in your DC.
    
7.  Select Next for Disk Space Capacity.
    
8.  Click Customize Hardware.
    
9.  Change Network Adapter to VMnet3
    
    ![image](https://github.com/user-attachments/assets/d7e492d5-c00e-4da6-b629-87fd8eb9e7b1)   
    
10.  Uncheck "Power on this Virtual Machine after Creation". Click Finish.
    
11.  Edit Virtual Machine Settings.
    
12.  Remove the Floppy Drive. Click OK.
    
13.  Install Windows 10.
    
   ![](https://framerusercontent.com/images/hKiTbgTxPoZEM1KuwHG1PXJFFs.png)    
14.  Accept Terms and License.
    
15.  Select Custom Install.
    
16.  Select New > Apply > OK > Next
    
  ![](https://framerusercontent.com/images/qAehGQdcgkkty1J2Y4Whx7OcHE.png)    
17.  Continue with Installation. Select "I don't have internet".
    
  ![](https://framerusercontent.com/images/WPYhSVQEKyEBurJ2qFACqqWqUI.png)    
18.  Continue with Limited Setup.
    
19.  Set the user as the same one you used for DC.
    
20.  Set security answers
    
21.  Uncheck all the privacy settings then select Accept.
    
  ![](https://framerusercontent.com/images/uoOTvI8InLx4eGwaPb2iSMPYaeY.png)    
22.  Choose "Not Now" for Cortana
    
23.  Inside VMware, click Player > Manage > Install VMware tools.
    
24.  Navigate to This PC > DVD Drive (D:) VMware Tools. Run the installation.
    
25.  Install the Complete Version.
    
26.  Reboot.
    
27.  Navigate to About > Rename this PC.
    
28.  Reboot.
    
29.  Navigate to Network and Internet Settings > Change Adapter Options.
    
30.  Right Click on Ethernet0 > Properties > Internet Protocol Version 4 (TCP/IPv4) > Use the following IP Address. Enter the following IP Addresses.
    
  ![](https://framerusercontent.com/images/Ly677VlFu1ScWbOusPWxixy81uk.png)    

**Join the PC's to the Domain:**

1.  Launch Windows Server.
    
2.  Launch Command Prompt
    
3.  Type ipconfig and search for your IP Address.
    
    ![](https://framerusercontent.com/images/0JdFQH4PDfZhoeEdnCf9LyPLY.png)    
4.  Launch Kali Linux
    
5.  Login to pfSense Dashboard
    
6.  Navigate to Services >DHCP Server > Victim Network
    
7.  Add Windows Server IP Address to DNS servers.
    
    ![image](https://github.com/user-attachments/assets/620076a4-48ca-46e2-95a0-fb839e40934e)   
    
8.  Enter your domain name from your domain controller.
    
    ![image](https://github.com/user-attachments/assets/0e3a0393-256e-4947-a32f-ca6bed82cd62)   
    
9.  Launch Windows 10.
    
10.  Navigate to Access work or school > Connect > Join this device to a local Active Directory domain. Add your domain.
    
   ![image](https://github.com/user-attachments/assets/a3379392-b5b8-4e64-910f-8df66dc4ca1e)   
    
11.  Set up account info. (Use the same password)
    
  ![image](https://github.com/user-attachments/assets/ff550bdf-55b2-48ce-8ee9-45369d0c50b2)   
   
12.  Skip add account.
    
13.  Once finished Restart.
    
14.  Inside Windows Server, navigate to Server Manager Dashboard > tools > Active Directory Users and Computers. Confirm that you have added your computer.
    
   ![image](https://github.com/user-attachments/assets/d2ad1d24-45a1-4da6-8080-a59fa334e6dd)   
   

![image](https://github.com/user-attachments/assets/0274f634-ef3c-4cb5-81b1-66f56ac58489)   
   

## **Configuring Splunk on Ubuntu Server**

Setting up Splunk within a VMware virtual machine is an important step in creating a cybersecurity environment that's both robust and flexible. Splunk helps analyze large amounts of data, such as security logs, making it a vital tool for security monitoring. By configuring Splunk in VMware, you're creating a controlled space where it can run without affecting other parts of your computer system. This approach allows for more focused monitoring, efficient use of resources, and easier scalability as your needs grow. It's a balanced solution that meets the demands of both novice users and seasoned professionals, providing a streamlined path to enhanced cybersecurity monitoring. We will be using an Ubuntu Server for our Splunk instance.

1.  [Download Ubuntu Server here.](https://ubuntu.com/download/server) (Manual Server Installation)
    
2.  Launch VMware.
    
3.  Create a New Virtual Machine.
    
4.  Select the path to your Ubuntu Server.
    
5.  Name your virtual machine.
    
    ![image](https://github.com/user-attachments/assets/b774cec6-92cf-4116-83fc-2b82c095ac10)   
   
6.  Select 100GB disk space and store single file.
    
7.  Click Customize Hardware.
    
8.  Click Add Network Adapter. Custom to VMnet6. Remove USB controller, sound card, and printer. Click Close.
    
9.  Click Finish.
    
10.  Launch Ubuntu Server.
    
11.  Enter Language.
    
12.  Click Enter for Network.
    
13.  Click Enter for no proxy.
    
14.  Click Enter for guided storage configuration, use an entire disk.
    
15.  Click Enter for file system summary.
    
16.  Click to Continue.
    
  ![image](https://github.com/user-attachments/assets/f74dbb96-6351-4766-8c86-62e43017317e)
   
17.  Enter your profile setup.
    
   ![image](https://github.com/user-attachments/assets/635a3898-d3df-43fd-b1f4-3b4bf24f3ebf)   
    
18.  Install OpenSSH Server. Make sure it is checked.
    
19.  Select your server snaps.
    
20.  Click Done to start installation.
    
21.  Reboot.
    
22.  Login with your credentials.
    
23.  Type in sudo apt install tasksel. Enter your password. Type y for yes.
    
   ![image](https://github.com/user-attachments/assets/686c55eb-129a-4b65-989f-1b5e3384a9f3)   
   
24.  Type sudo apt install ubuntu-desktop (If it's not download try running; sudo apt update then try again)
    
25.  Reboot.
    
26.  Launch Firefox.
    
27.  Go to the [Splunk](https://www.splunk.com/) website.
    
28.  Click on Free Splunk.
    
  ![image](https://github.com/user-attachments/assets/113a7a6c-d97d-4fc0-95e1-f439dca3874e)   
    
29.  Create a free account, I used mail.com for my email.
    
30.  Login with your credentials.
    
31.  Navigate to [Splunk Enterprise](https://www.splunk.com/en_us/download/splunk-enterprise.html) and Choose Free Trial.
    
32.  Download the Linux .tgz version.
    
33.  Launch Terminal.
    
    
   ![image](https://github.com/user-attachments/assets/06bf1a27-81c5-4b07-a0a8-2d1e6942c8d3)   
    
34.  Type **tar xvzf** and your splunk file.
    
   ![image](https://github.com/user-attachments/assets/0ac24822-37b2-4363-b889-5c70afa9d307)   
    
35.  Type ls
    
36.  Type cd splunk
    
37.  Type ls
    
38.  Type cd bin
    
39.  Type ./splunk start
    
40.  Scroll down and click y for yes.
    
41.  create an administrator account with username and password.
    
42.  take note of the splunk web interface.
    
  ![image](https://github.com/user-attachments/assets/49245e23-2c58-47c0-b8c4-4854136e2753)   
   
43.  Launch your browser and enter http://splunk:8000
    
44.  Login with your credentials.
    
  ![image](https://github.com/user-attachments/assets/1d53f1d1-d7f7-4e4b-b7db-4dc10550940f)   
    

## **Installing a Universal Forwarder**

1.  Launch your Ubuntu VM and go to your splunk web interface.
    
2.  Navigate to settings > forwarding and receiving > configure receiving.
    
3.  Click New Receiving Port.
    
4.  Type **9997** on Listen on this port, which is the universal forwarder for splunk. Click Save.
    
5.  Navigate to settings > indexes > New Index.
    
6.  Name your index log wineventlog. Click Save.
    
7.  Launch Windows Server 2019.
    
8.  Open internet explorer.
    
9.  Go to tools > internet options > security tab > Custom level > Downloads > Enable.
    
10.  Before going to the next step, we have to create a new firewall rule to allow internet in your domain controller. Go to your pfSense web interface > firewall > rules > DC (victimnetwork) > add with down arrow > change protocol to any. Save and Apply Changes.
    
11.  In your Windows Server 2019 VM, download and install google chrome.
    
12.  Launch google chrome.
    
13.  [Download the Splunk Universal Forwarder.](https://www.splunk.com/en_us/download/universal-forwarder.html?)
    
14.  Install Splunk Universal Forwarder.
    
  ![](https://framerusercontent.com/images/DC2cHgOBdWlGkERxYFvD1jhNCw.png)    ![image](https://github.com/user-attachments/assets/31dc1527-0fe3-4bee-a4fe-f4f4419f4e4c)   
   
15.  Get your IP address from splunk instance in terminal by typing ifconfig.
    
  ![image](https://github.com/user-attachments/assets/1036fa4b-95cf-4a24-99b1-35024358c94e)   
    
16.  Use your splunk IP address.
    
  ![image](https://github.com/user-attachments/assets/195a97cf-16c3-49e9-929a-df5d1fe9abe8)   
    ![image](https://github.com/user-attachments/assets/2fce3326-58c7-4c0a-b980-9e9ebeada878)   
    
17.  Click install.
    
18.  Navigate to your splunk instance > settings > Add data > Forward.
    
   ![](https://framerusercontent.com/images/LceKL555L5dFWzZz6HpMDFewhKE.png)    
19.  Click on your domain controller and name your domain controller. Click Next.
    
   ![image](https://github.com/user-attachments/assets/a3834535-359f-4458-8424-0b6d04685ef9)   
    
20.  Navigate to Local Event Logs and select all of the event logs. Click Next.
    
   ![](https://framerusercontent.com/images/GHLsKoobePCa7as2CR6WU8knKpg.png)    
21.  Choose wineventlog in index.
    
   ![](https://framerusercontent.com/images/CFefquJ1nArlJMipqBxmyJ4Js.png)    
22.  Click Review and Submit.
    
23.  You can now view all your logs in splunk.
    

## **Conclusion**

We began by building our Host PC, which served as the foundation for all our operations. We set up VMware Workstation as our Hypervisor, allowing us to run multiple virtual machines simultaneously on a single system.

Next, we configured a pfSense Firewall, a crucial security measure that helped us divide our network into segments and maintain strict security controls over them. Following this, we installed Security Onion, our comprehensive solution for intrusion detection, security monitoring, and log management.

We also prepared Kali Linux, a potent tool designed for conducting penetration testing and security audits. Meanwhile, our Windows Server was established as a Domain Controller, managing user interactions and access to shared resources within our network.

In addition to these, we set up Windows-based desktops and configured Splunk, a platform that enabled us to search, monitor, and analyze machine-generated data from websites, applications, servers, and mobile devices.

Finally, we set up a universal forwarder in splunk. Using a Universal Forwarder in Splunk is crucial for your cybersecurity homelab. It's like a helper that grabs data from different areas of your lab such as firewalls or servers, and sends it to Splunk. This lets you check your network's safety by reviewing the collected data. Using it transforms your homelab into a real-world cybersecurity practice field, aiding you in spotting any suspicious and fixing security problems.
   You can try some additional installations which are optional

## **Optional: Installing ParrotOS**
