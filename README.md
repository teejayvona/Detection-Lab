
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
- Wireshark - Employed as a network analysis tool, Wireshark enables the capturing and examination of network traffic for the identification of potential security threats and performance issues.
- Atomic Red Team - Leveraged for telemetry generation, Atomic Red Team provides tools to simulate realistic network traffic and attack scenarios, aiding in the validation of security controls and detection capabilities.
- Active Directory - Simulated real-world centralized directory service responsible for overseeing user account management, group memberships, and access permissions throughout the network.

## Steps

### Ref 1: Network Diagram

![AD Logical Diagram](https://github.com/teejayvona/Detection-Lab/assets/33003865/c2b1b5b5-a729-4eca-8970-3be2d6353009)

This is the network diagram of what I intend achieving at the end of the project
All these was done using Virtual Box

### Ref 2: Sysmon Configuration File

I made use of Olaf Hartong configuration for my sysmon configuration

https://github.com/olafhartong/sysmon-modular

### Ref 3: Splunk inputs.conf

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
