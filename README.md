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

*2. Validating command and control (C2) access*

<img width="965" height="265" alt="image" src="https://github.com/user-attachments/assets/118c5ce8-ede8-4700-a1b2-587b9c5553ca" />


After successful execution of the payload, a C2 session was established between the compromised endpoint and the attacker machine. Active sessions were verified using the sessions command within the C2 framework, confirming remote control over the victim host.

*3. Identifying the LSASS process*

<img width="1906" height="446" alt="Screenshot 2026-06-26 154605" src="https://github.com/user-attachments/assets/5ac5b15c-40da-4420-aeec-7e61311f938f" />

The above image shows the attacker enumerating the lsass.exe process on the compromised Windows endpoint using the ps -e lsass.exe command. The process ID (PID) is required for the next stage of the attack, where the attacker targets the Local Security Authority Subsystem Service (LSASS) to extract credentials stored in memory.

*4. Dumping the LSASS process memory*

<img width="1672" height="137" alt="image" src="https://github.com/user-attachments/assets/13dbaa87-22be-426f-8804-15fed375bd2a" />

The above image shows the attacker using the legitimate Windows utility rundll32.exe together with comsvcs.dll to create a memory dump of the LSASS process. This Living-off-the-Land Binary (LOLBIN) technique abuses trusted Windows components to extract credentials from memory while attempting to avoid detection by traditional security solutions.

*6. Investigating sensitive process access in LimaCharlie*

<img width="1577" height="852" alt="image" src="https://github.com/user-attachments/assets/eb971275-7388-495f-930b-c308ff38782a" />

The above image shows the investigation of endpoint telemetry within LimaCharlie after the credential dumping attack. The Timeline was filtered for SENSITIVE_PROCESS_ACCESS events involving rundll32.exe, allowing the suspicious activity to be isolated. This event indicates that rundll32.exe attempted to access the memory of the sensitive lsass.exe process, a behavior commonly associated with credential dumping techniques and frequently monitored by Endpoint Detection and Response (EDR) solutions.

*7. Creating a detection rule for LSASS credential dumping*

<img width="1577" height="852" alt="image" src="https://github.com/user-attachments/assets/8dcbdfc4-57c9-4df6-ace1-86778fd5511e" />

The above image shows the creation of a LimaCharlie Detection & Response (D&R) rule designed to identify unauthorized access to the lsass.exe process. The detection monitors SENSITIVE_PROCESS_ACCESS events targeting lsass.exe while excluding wmiprvse.exe to reduce false positives. When the rule conditions are met, LimaCharlie generates a detection report named LSASS access, providing analysts with immediate visibility into potential credential dumping activity.

*8. Validating and deploying the LSASS detection rule*

<img width="1917" height="867" alt="image" src="https://github.com/user-attachments/assets/8e000318-04da-42da-9dbd-89aeb41b8cb4" />

The above image shows the validation and deployment of the LSASS credential dumping detection rule in LimaCharlie. The rule was tested against historical telemetry using the built-in “Target Event” function, where it successfully returned a Match, confirming that the detection logic correctly identifies SENSITIVE_PROCESS_ACCESS events targeting lsass.exe.

After validation, the rule was saved as LSASS Accessed, officially deploying it within the environment. This ensures that any future attempts to access LSASS memory will trigger an alert, enabling real-time detection of credential dumping activity.

