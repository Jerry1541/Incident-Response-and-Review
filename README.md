# Incident-Response-and-Review

## Objective

Participated in Clicked IBM Skillsbuild Mini Sprint session on incident response and related activities. 
Completed incident response and review with a team in different time zones and achieve deliverables and valuable experience. 
Working on a scenario with healthcare org to protect its data and network from cyberattacks.

### Skills Learned

- Incident identification and investigation of attack taken place (going over IPs and logs to determine attacks)
- Peruse and become familiar with Windows Event logs and attacks (brute-force, unauthorized rdp, lateral movement)
- Develop short-term and long-term plans for incident response containment and eradication
-	Communicate incident and prepare documentation to C-level executive
-	Soft skills in communicating security incidents to upper management

### Tools Used

-	Eric Zimmerman’s Timeline explorer
-	Bulk ip lookup tool

## Steps

<p align="center">
    <img src="https://i.imgur.com/0a4D9fz.png" height="50%" width="50%" alt=""/> <br/>
    Data flow and system architecture <br/>
</p>

### Identification and Investigation
Timeline based on Windows Event Logs:

<table>
  <th>Log</th>
  <th>Time (EDT)</th>
  <th>Event ID</th>
  <th>Attack</th>
  <th>Notes</th>
  <tr>
    <td>3</td>
    <td>08:10:23</td>
    <td>4624</td>
    <td>Successful logon</td>
    <td>By JohnDoe into DESKTOP-1234567 from 192.168.1.2:50215, logon type 10 (RDP)</td>
  </tr>
  <tr>
    <td>5</td>
    <td>09:45:32</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
				
Date: 2023-09-20

Timezone: New York, EDT (GMT-4), based on Maven clinic HQ location

Tools like Timeline Explorer helped clear the fog from the data set to make better deductions of what took place during the attack:











Log Analysis:
-	Upon using the showmyip bulk IP lookup tool, the IPs geolocations are visualized on the world map to see where the requests are from:














-	Using these locations, we can determine whether it is legitimate network requests or not
-	Tool like Abuseipdb showed specific IPs which may have been used in prior attacks:











Affected systems, services:
-	DESKTOP-1234567
-	MSSQLSERVER database file, “mydatabase.mdf”
-	Policy changed on DC-SERVER-01
-	DNS tracking on SERVER-12345
Patterns/Anomalies:
-	Lateral movement from using logon type: 3 (failed and successful attempts)
-	Brute force attacks from logs 10, 11, 12
-	Unauthorized access into DESKTOP-1234567 using RDP and log 12
-	Privilege escalation
Questions for stakeholders:
-	Is there a list of assets (inventory) that can be accessed to understand potential impact of system? I.e, where can lateral movement take the attacker.

//next page














Response Containment and Eradication
Short-term plan: (Snapshot, Isolate comprised systems, uninstall malware, revert changes, traffic analysis)
Actions	Steps to take
Snapshot	Take snapshot of current system to take closer look at changes and unauthorized access, modifications etc.
Isolate systems	Isolate data and systems like DESKTOP-1234567, SERVER-12345, DC-SERVER-01, SQLSERVER-12345
	Isolate the SQL database and use backup until resolved
Uninstall/remove unwanted software	Uninstall software unknown.exe and all data related to it
	Remove tracking application on SERVER-12345
Make changes	Revert changed policy on Domain controller
	Change JohnDoe account password and access
	Mandatory password reset on all accounts
Traffic analysis	Block/filter traffic from identified malicious IP addresses involved in the attack







Long-term plan: (Further investigation, monitoring, improved access controls, user education)
Actions	Steps to take
Further investigation	Identify how credentials were acquired by JohnDoe (data leak?)
	Identify how to prevent privilege escalation to Admin account via User and Groups
	Plan how to prevent lateral movement of attackers once access is gained to network
Monitoring	Continue monitoring network for malicious activity
Improved access controls	Firewall rules changed need to be reverted
	Implement stringent intrusion detection systems
	Enable MFA for database access
	Update all software on SERVERS and apply necessary patches
Education	User education and training to prevent credential leak

Overview:
-	Inform authorities and patients on such a security gap
-	Unauthorized access of network can mean PII data of patients has been compromised
-	Ensure compliance with HIPAA, PCI DSS, GDPR (legal team)
-	Prevent any more downtime of servers or database (which has patient records)
-	Consider company branding and image (PR)
Cost-analysis:
-	Tools
o	IDS/IPS - $10,000 to $50,000
-	User education & training - $30 to $100 per employee annually
-	Business disruptions/downtime (SQL database, patient details) - $300,000+
-	Penalties involved with compliance violations
-	Cybersecurity insurance

Presentation to CTO:
Affected systems have been isolated to prevent spread of attack
Eradicated malware and resolved vulnerabilities
Policy changes are required
Continue to monitor for further network security events
Provide user education and training on security incidents
Implement improved access controls
Perform further investigation
Get legal team involved for compliance and regulations (HIPAA, PII)
Consider potential company branding as a result of security incident
Decide on financial impact and decisions to be made

Post Incident Review
Target audience:
-	Upper management interested in financial impacts and business objectives
-	Affected business units interested in how to prevent such security incidents
-	Considering that the security gap caused violations of HIPAA and GDPR, legal team and PR will have to be involved in this meeting.
Timeline:
-	Security incident occurred on 2023-09-20 from 8am to 6pm. 
-	Attacker gained access to network and activities were logged by security controls in place, including potential data exfiltration.
-	Once alarms were raised from this security incident, the security team conducted initial investigations and containment of the attack on 2023-09-21.
-	Affected systems and services (including patient records) were restored on 2023-09-22, and necessary access control measures deployed.
Impact to business:
-	Loss of trust in customers (patients)
-	Compliance violations and fees involved with that
-	Downtime of affected systems and business lost as a result of that

Security Review:
What went right:
-	Immediate detection and response to security incident
-	Proper protocols and policies followed to respond and contain the attack and consequences
What could be better:
-	More hands-on deck to respond to such incidents
-	Implementation of SOAR and SIEM tools to detect and respond faster
-	To prevent such re-occurrences improve overall security posture by reducing attack vectors and surfaces for potential vulnerabilities
-	Move forward with more user training, employ concepts of defense-in-depth and least privilege.
Lessons learned:
-	Security incidents can occur even when mitigative strategies and access management tools are in place, ie, ensure cyber hygiene at all times (do not let guard down)
-	Incidents such as these can have impacts not just on affected systems but also the end user (patients and compliance)
The future:
-	Hire external security consultation company to recommend potential security measures
-	Consider cybersecurity insurance plan in place for security incidents
-	In the event of security events to have business continuity and risk management plans

 
 
## References
<a href="https://www.clicked.com/learning-experience-page/incident-response-and-review-mini-sprint-9-23-24">Clicked Incident Response and Review Mini Sprint</a>
