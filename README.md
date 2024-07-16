
# Active Directory Project

## Objective

The goal of the Detection Lab project was to create a regulated setting to simulate and identify cyber threats. It concentrated on processing and examining logs using a Security Information and Event Management (SIEM) system, producing simulated data to replicate actual attack scenarios. This practical engagement was crafted to enhance comprehension of network security, attack methodologies, and defensive tactics.

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Splunk - Utilized as a Security Information and Event Management (SIEM) platform, Splunk facilitates the ingestion and analysis of logs for comprehensive security monitoring and incident detection.
- Atomic Red Team - Leveraged for telemetry generation, Atomic Red Team provides tools to simulate realistic network traffic and attack scenarios, aiding in the validation of security controls and detection capabilities.
- Active Directory - Simulated real-world centralized directory service responsible for overseeing user account management, group memberships, and access permissions throughout the network.

## Steps

### Ref 1: Network Diagram

![AD Logical Diagram](https://github.com/teejayvona/Detection-Lab/assets/33003865/4524a778-a1b8-43f1-b488-77ec836bb6a2)


This is the network diagram of what I intend achieving at the end of the project
All these was done using Virtual Box

### Ref 2: Sysmon Configuration File

I made use of Olaf Hartong configuration for my sysmon configuration

https://github.com/olafhartong/sysmon-modular

### Ref 3: SplunkForwarder Installation using a custom inputs.conf

After installing splunk forwarder to listen to my splunk server on 192.168.10.10 on the default port 9997. We need to configure the telemetries to send by opening a Notepad with administrator rights. The code below was then insert and saved on the splunk forwarder local file as "inputs.conf"

[WinEventLog://Application]

index = endpoint

disabled = false

[WinEventLog://Security]

index = endpoint

disabled = false

[WinEventLog://System]

index = endpoint

disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]

index = endpoint

disabled = false

renderXml = true

source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational

### Ref 4: Configure Splunk

Log onto 192.168.10.10:8000 with correct details. Go to settings and then indexes to "endpoint" as a new index

Also from setting, select forwarding and listening and add a new port for listening. The listening port is 9997

You might experience a little technical issue like me as below.

![VirtualBox_Windows 10_16_04_2024_21_48_00](https://github.com/teejayvona/Detection-Lab/assets/33003865/48ff714c-7bc9-4139-bccf-9b11e45ef170)

This can be fixed by changing directory on the Splunk server to:

cd /opt/splunk/etc/system/default/

sudo vim server.conf

![VirtualBox_Splunk Server_16_04_2024_22_39_07](https://github.com/teejayvona/Detection-Lab/assets/33003865/7c76c8dc-560f-43ff-a70b-3c7f98c09ebd)

cd /opt/splunk/etc/system/local/
sudo vim server.conf

or better still just paste this on cd /opt/splunk/etc/system/local/

[diskUsage]
minFreespace = 50
pollingFrequency = 100000
pollingTimeFrequency = 10

and that would be sorted

### Ref 4: Win10 Pro and ADD Server Installation
I installed Windwos 10 Pro and ADD Server Desktop edition to use as our taget system and Domain server respectively. 

I then manually added users to the ADD to mimic real users in an organisation. 

Splunk universal forwarder and Sysmon were install on both systems with the addition of ART on Win10 pro only to simulate attack and investigate telemetries.

The users were also given remote desktop rights

### Ref 5: Kali Linux
Kali was installed on VB from Kali official site. We immediately did update on the repositories and eventually installed crowbar which would be used to perform bruteforce attack.

Crowbar comes with a rockyou.txt file contain a bunch of password

By using nano head -n 20 and pointed to password.txt we created a password list for the first 30 to reduce attempt time

We them assume the username 'jadone' has been compromised and type the following for remote desktop brute force attempt

crowbar -b rdp -u jadoe -C password.txt -s 192.168.10.100/32


### Ref 6: View Temeletry on Splunk
Login on to Splunk, we notice difference event ID as well as event ID 4625 which for failed login attempt

Going deeper, we notice all the attempts occured within a minute and the ip is from outside our network
