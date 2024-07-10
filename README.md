# ActiveDirectoryHomeLab



<h2>Description</h2>
In this project I will create an active directory to manage resources: users, computers, and groups. I will use splunk instance that will injest events from windows server that has the active directory and target windows machine. This project will practice red team skills of brute force attacks to see what telementry it generates. I want to give a big thanks to Steve at MyDFIR for the project idea and how the system works. I will get kali linux skills as well as splunk and how to use it effectively. This write up is not a step by step guide.
<br />
<br />

What is an Active Directory? It is a database that contains users, computers, and groups etc. It is a critical component for network security and management in enterprise environments. In cybersecurity (AD) Active Directory plays a pivitol role in authentication/authorization, management, group policy, and directory based security.
<br />




<h2>Environments Used</h2>

-<b> Windows 10 </b>
-<b> Kali Linux </b>
-<b> Windows Server </b>
-<b> Splunk </b>



<h2>Program Walk-through</h2>

<h3> Network Diagram</h3>
<img width="300" height="450" alt="Screenshot 2024-07-08 at 4 53 12 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/d404c082-bdb4-452a-83dc-fc6c0717618d">

We use this diagram to set up our lab testing enviornment network variables. 
</br>

<h3>Setting up Static IP on the splunk server</h3>
In order to set this up on the splunk server vm, I had to use the command: sudo nano /etc/netplan/00-installer-config.yaml. This file was the only one in the etc folder. Which brought me up to this screen which allowed me to set my static IP to the one in the diagram. I am currently in the Network layer 3 of the OSI model by configuring all of the static IP addresses.
</br>
<img width="450" height="450" alt="Screenshot 2024-07-09 at 12 26 55 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/8b2cee7e-a062-4200-a958-0b7839b9c881">
</br>
I had to use command sudo netplan apply to finalize all changes. 
