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

 <h2> Setting up Static IP on the splunk server </h2>
In order to set this up on the splunk server vm, I had to use the command: sudo nano /etc/netplan/00-installer-config.yaml. This file was the only one in the etc folder. Which brought me up to this screen which allowed me to set my static IP to the one in the diagram. I am currently in the Network layer 3 of the OSI model by configuring all of the static IP addresses.
</br>
<img width="450" height="450" alt="Screenshot 2024-07-09 at 12 26 55 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/8b2cee7e-a062-4200-a958-0b7839b9c881">
</br>
I had to use command  sudo netplan apply to finalize all changes. 
</br>
<img width="1101" alt="Screenshot 2024-07-10 at 4 02 24 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/25bb4988-1564-4ea5-a836-1a0c3c8568f8">
</br>
This will run with the user splunk is online everytime. Then I wanted to make sure my vm-host-pc is now renamed to target-pc which will distinguish itself in the labs.
</br>
<h3>TargetPC Config Splunk Indexer and Sysmon</h3>
<img width="753" alt="Screenshot 2024-07-10 at 4 10 24 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/e5a14504-1b2d-4f8a-8699-778723f4f4f4">
</br>
The target pc had the same IP address as the windows server in the diagram I had set up. I had to go into the network adapter settings to configure it as a static IP. This will prevent any IP conflicts from happening. 
</br>
<img width="557" alt="Screenshot 2024-07-10 at 4 18 12 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/ed88a102-ffe2-470e-bea0-ce4a15603058">
</br>
One of the main problems I ran into was the account creation for splunk enterprise. It took two weeks fo non stop calling to customer service to resolve the "48 hour wait time" specified for trial email. After the problem was resolved I was able to download splunk and indexer, which I made sure to put the correct IP address of the target-Pc.
</br>
<img width="829" alt="Screenshot 2024-07-10 at 4 35 27 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/3647c99c-f96f-47c5-9d8d-8334ccf46fde">
</br>
These configurations are instructing the splunk forwarder to push events releated to applications, security, and sysmon over to the splunk server. All the indexes are pointing to the endpoints which any event from  the endpoint will be pushed into our splunk server. Thanks to youtuber steve for providing this part in the homelab.
</br>
<img width="1034" alt="Screenshot 2024-07-10 at 5 06 38 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/64481b99-b438-4e65-b5a8-f4bd1e50536f">
</br>
The indexes on splunk did not have endpoint which needed to be created since we updated the local directory files with the config file for endpoints. The port that the new index needed to listen on was 9997. After clicking save, I was already able to see events from my target-pc. I made a search for index=endpoint (last 24hours)
<img width="1026" alt="Screenshot 2024-07-10 at 5 10 14 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/4b2e5a0f-88f2-4e1e-a6ae-4138bb5fe7f6">
</br>

<h2>Configuring the windows server VM</h2>
<img width="910" alt="Screenshot 2024-07-10 at 5 24 59 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/fc7a6126-36a6-442f-9902-c474baa89535">
</br>
I had to configure the server to have the correct static IP to prevent any errors into our active directory. Once it was configured I used ipconfig command to check the static ip as well as ping google.com for connectivity.
</br>
<img width="1133" alt="Screenshot 2024-07-10 at 5 29 41 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/5c7eda48-1002-4e85-a76c-f96a1f7686cd">
</br>
I had to promote this server to a domain controller by adding it to a new forest calling it mydfir.local with a password. I learned that attackers love to target domain controllers since it has access to everything which contains password hashes. Any activity report in this file would mean the server has been compromised.
</br>
<img width="724" alt="Screenshot 2024-07-10 at 5 34 40 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/71a598e0-5aa3-4e28-8289-34de4ec82179">
</br>
<img width="845" alt="Screenshot 2024-07-10 at 5 36 34 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/15968129-20ba-4cb5-87c2-ca1722025eaa">
</br>
I had to set up some test departments and fill in some users into the correct groups. In a higher enterprise this would be broken up by departments.
</br>

<h2>Target PC To Join The Domain and authenticate with new account</h2>

<img width="778" alt="Screenshot 2024-07-10 at 5 43 45 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/f7adeb75-7634-45cd-a901-057a5be41c1c">
</br>
The error from target machine does not know how to resolve the mydfir.local domain. I had to change the network adapter properties to change IPv4.
</br>
<img width="423" alt="Screenshot 2024-07-10 at 5 48 14 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/b9d8055a-fb37-4dd8-81c5-5cc87b643650">
</br>
<img width="1100" alt="Screenshot 2024-07-10 at 5 49 44 PM" src="https://github.com/Developer-AaronB/ActiveDirectoryHomeLab/assets/91814805/ce7ac2c9-fdad-4ec1-91b2-29263e959d6e">
</br>
I had to restart the target pc and now select other user while making sure it pointed to the domain mydfir. I logged in with the test account and now we have two machines talking with each other. It will pass events to our splunk server now. 










