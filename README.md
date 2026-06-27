# edr-c2-detection-lab

## Objective


This detection Lab project aimed to simulate a command and control (C2) attack on a Windows endpoint and analyze how endpoint detection and response telemetry from LimaCharlie identifies malicious process execution, network behavior, and compromise indicators in a controlled lab environment.
### Skills Learned


- C2 attack simulation and endpoint compromise analysis using LimaCharlie
- Process tree analysis and malicious execution detection on Windows systems
- Network and endpoint telemetry correlation for intrusion investigation
- Blue team incident detection, validation, and response workflows
- Hands on understanding of post exploitation behavior in controlled environments

### Tools Used


- LimaCharlie – Endpoint Detection and Response (EDR) platform used to monitor endpoint telemetry, detect suspicious activity, and investigate the attack.
- Sliver – Used to generate the payload and establish a command and control (C2) session.
- Windows – Victim operating system where the malicious payload was executed.
- Kali Linux – Attacker machine used to host the C2 server and interact with the compromised endpoint.

  
## Attack Flow
- Victim downloads the malicious executable.
- Victim executes the payload.
- Payload establishes a C2 connection.
- Attacker validates access.
- LimaCharlie detects suspicious activity.
- Investigation is performed using EDR telemetry.
  

## Steps

*1. Victim downloads an executable file*


<img width="967" height="868" alt="Screenshot 2026-06-26 154640" src="https://github.com/user-attachments/assets/ea50f2b2-7785-4c60-a2cf-128495c93001" />


The above image shows the victim downloading a malicious executable file named ANCIENT_CARDIGAN.exe after accessing a malicious link. Once executed, the file initiates the attack by establishing a foothold on the victim's Windows system, serving as the initial compromise that enables the subsequent command and control (C2) communication.

*2. Validating command and control (C2) access *


<img width="1906" height="446" alt="Screenshot 2026-06-26 154605" src="https://github.com/user-attachments/assets/5ac5b15c-40da-4420-aeec-7e61311f938f" />


After successful execution of the payload, a C2 session was established between the compromised endpoint and the attacker machine. Active sessions were verified using the sessions command within the C2 framework, confirming remote control over the victim host.

