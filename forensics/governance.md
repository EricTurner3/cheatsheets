# Rules, Procedures, Standards & Methodologies

1. [Rules of Evidence](#rules-of-evidence)
1. [Forensics Readiness Planning](#forensics-readiness-planning)
1. [Ensuring Quality Assurance](#ensuring-quality-assurance)
1. [First Response by Laboratory Forensics Staff](#first-response-by-laboratory-forensics-staff)
1. [Computer Forensics Methodology](#computer-forensics-methodology)
1. [Order of Volatility](#order-of-volatility)
1. [Data Acquisition Methodology](#data-acquistion-methodology)
1. [Windows Forensics Methodology](#windows-forensics-methodology)
1. [Guidelines to Ensure Log File Credibility and Usability](#guidelines-to-ensure-log-file-credibility-and-usability)
1. [Forensic Acquisition of Amazon EC2 Instance: Methodology](#forensic-acquisition-of-amazon-ec2-instance-methodology)
1. [Forensic Acquisition of VMs in Azure: Methodology](#forensic-acquisition-of-vms-in-azure-methodology)
1. [Container Forensics and Incident Response: Methodology](#container-forensics-and-incident-response-methodology)
1. [Steps to Investigate Email Crimes](#steps-to-investigate-email-crimes)
1. [Preparing Testbed for Malware Analysis](#preparing-testbed-for-malware-analysis)
1. [Dynamic Malware Analysis: Pre-Execution Preparation](#dynamic-malware-analysis-pre-execution-preparation)
1. [Mobile Forensics Process](#mobile-forensics-process)

## Rules of Evidence
Digital evidence must follow the following to make it admissible in a court of law
1. Understandable - clear and understandable to judges
2. Admissible - must be related to the fact being proved
3. Authentic - must be real and related to the incident
4. Reliable - must be no doubt about the authenticity or veracity of the evidence
5. Complete - must prove the attacker's actions or innocent

## Forensics Readiness Planning
1. Identify potential evidence required
1. Determine the source of evidence
1. Define a policy that determines the pathway to legally extract electronic evidence with minimal disruption
1. Establish a policy to handle and store the acquired evidence in a secure manner
1. Identify if the incident requires full or formal investigation
1. Create a process for documenting the procedure
1. Establish a legal advisory board to oversee
1. Keep an incident response team ready to review and perserve evidence

## Ensuring Quality Assurance
1. Arrange formal, documented trainings
1. Validate equipment and document it
1. Conduct annual proficiency test for investigators
1. Follow appropriate standards and/or controls in casework
1. Have policies and procedures in place for effective forensic investigations
1. Attaiin ASCLD/LAB accreditation and/or ISO/IEC 17025 accreditation
1. Perform quality audits and qualit management system review
1. Ensure physical plant security
1. Assure health and safety
1. Review, update, and document policy standards annually

## First Response by Laboratory Forensics Staff
1. Documenting the Electronic Crime Scene
1. Collecting Incident Information
1. Planning the Search and Seizure
1. Identifying and Collecting Electronic Evidence
1. Packaging Electronic Evidence
1. Transporting Electronic Evidence

## Computer Forensics Methodology 
1. Documenting the Electornic Crime Scene
1. Search and Seizure
1. Evidence Preservation
1. Data Acquistion
1. Data Analysis
1. Case Analysis
1. Reporting
1. Testify as an Expert Witness

## Order of Volatility
1. Registers and cache
1. Routing table, process table, kernel statistics, and memory
1. Temporary system files
1. Disk or other storage media
1. Remote logging and monitoring data that is releavant to the system in question
1. Physical configuration, and network topology
1. Archival media

## Data Acquistion Methodology
1. Determine the Data Acquisition Method
1. Select the Data Acquisition Tool
1. Sanitize the Target media
1. Acquire Volatile Data (if computer is already on)
1. Turn off the computer (if computer is already on)
1. Remove the Hard Disk
1. Write protect the suspect device
1. Acquire non-volatile data
1. Plan for Contigency
1. Validate Data Acquisition

## Windows Forensics Methodology
1. Collecting Volatile Information
1. Collecting Non-Volatile Information
1. Windows Memory Analysis
1. Windows Registry Analysis
1. Cache, Cookie, and History Analysis
1. Windows File Analysis
1. Metadata Investigation
1. Event Logs Analysis

## Guidelines to Ensure Log File Credibility and Usability
1. Log Everything
1. Synchronize Time
1. Use Multiple Sensors
1. Missing Logs
1. Ensure System's Integrity
1. Control access to log file

## Forensic Acquisition of Amazon EC2 Instance: Methodology
1. Isolate the compromised EC2 instance from the production environment
1. Take a snapshot of the EC2 instance
1. Provision and launch a forensic workstation
1. Create evidence volume from the snapshot
1. Attach the evidence volume to the forensic workstation
1. Mount the evidence volume onto the forensic workstation

## Forensic Acquisition of VMs in Azure: Methodology
1. Create a snapshot of the OS disk of the suspect VM via Azure portal or Azure CLI
1. Copy the snapshot to a storage account under a different resource group where it can be stored for forensic analysis
1. Delete the snapshot from the source resource group and create a backup copy
1. Mount the snapshot on a forensic workstation

## Container Forensics and Incident Response: Methodology
1. Do not terminate the nodes/pods
1. Collect all relevant logs including OS, container and application-level
1. Locate the pod and its worker node whic includes the affected container
1. Quarantine the pod by creating a network policy that prevents inbound/outbound traffic to it
1. Cordon the node and drain it off other unaffected pods
1. Assign IAM roles to restrict the affected pod's access to other resources
1. Apply termination protection on the affected node
1. Tag the affected node with a label

## Steps to Investigate Email Crimes
1. Seizing the computer and email accounts
1. Acquiring the email data
1. Examining email messages
1. Retrieving email headers
1. Analyzing email headers
1. Recovering deleted email messages

## Preparing Testbed for Malware Analysis
1. Allocate a physical system for the lab
1. Install a virtual machine on the system
1. Install guest OSs in the virtual machines
1. Isolate the system from the network by ensuring that the NIC card is in host only mode
1. Simulate internet services using tools usch as INetSim
1. Disabled "shared folders" and the "guest isolation"
1. Install malware analysis tools
1. Generate hash value of each OS and tool
1. Copy the malware onto the forensic workstations

## Dynamic Malware Analysis: Pre-Execution Preparation
1. Create a fresh baseline of the forensic workstations
1. Take a snapshot of the registry keys, file system, running processes, and event log files
1. List all Windows services, device drivers and startup programs
1. Install tools that will capture the changes performed by the malware
1. Generate hash values of the OSs and the tools
1. Run the malware on the forensic workstation

## Mobile Forensics Process
1. Collect the Evidence
1. Document the Evidence
1. Preserve the Evidence
1. Forensic Acquisition and Analysis
1. Generate Report