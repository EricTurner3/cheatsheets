# Rules, Procedures, Standards & Methodologies

1. [Rules of Evidence](#rules-of-evidence)
1. [Federal Rules of Evidence](#federal-rules-of-evidence)
1. [Scientific Working Group on Digital Evidence](#scientific-working-group-on-digital-evidence-swgde)
1. [ACPO - Principles of Digital Evidence](#association-of-chief-police-officers-acpo---principles-of-digital-evidence)
1. [Forensics Readiness Planning](#forensics-readiness-planning)
1. [Ensuring Quality Assurance](#ensuring-quality-assurance)
1. [First Response by Laboratory Forensics Staff](#first-response-by-laboratory-forensics-staff)
1. [Computer Forensics Methodology](#computer-forensics-methodology)
1. [Order of Volatility](#order-of-volatility)
1. [Data Acquisition Methodology](#data-acquistion-methodology)
1. [Standards for Sanitizing Media](#standards-for-sanitizing-media)
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

## Federal Rules of Evidence
The full list can be found [here](https://www.law.cornell.edu/rules/fre).
* Rule 102: Purpose
* Rule 103: Rulings on Evidence
* Rule 104: Preliminary Questions
* Rule 105: Limited Admissibility
* Rule 801: Hearsay Rule / Statements That Are Not Hearsay
* Rule 803: Hearsay Exceptions - Availability of Declarant Immaterial
* Rule 804: Hearsay Exceptions; Declarant Unavailable

## Scientific Working Group on Digital Evidence (SWGDE)
* Principle 1 - Ensuring accuracy and reliability of evidence during collection, preservation, examination and transference
* Standards and Criteria 
    * 1.1 - All agencies that seize and/or examine digital evidence must maintain an appropriate Standard Operating Procedure (SOP) document
    * 1.2 - Agency management must review the SOPs on an annual basis
    * 1.3 - Procedures must be gathered and recorded in a scientific manner or gennerally accepted in the field
    * 1.4 - Agency must maintain written copies of appropriate procedures
    * 1.5 - Agency must use appropriate & effective hardware and software for seizure or examination
    * 1.6 - All activity must be recorded in writing and available for review and testimony
    * 1.7 - Any action that has the potential to alter, damage or destroy any aspect of the original evidence must be performed by qualified persons in a forensically sound manner.

## Association of Chief Police Officers (ACPO) - Principles of Digital Evidence
1. Principle 1: No action taken by law enforcement agencies or their agents should change data held on a computer or storage media which may subsequently be relied upon in court
1. Principle 2: In exceptional circumstances, where a person finds it necessary to access original data held on a computer or on storage media, that person must be competent to do so and be able to explain his/her actions and the impact of those actions on the evidence, in court.
1. Principle 3: An audit trail or other record of all processes applied to computer based electronic evidence should be created and preserved. An independent third party should be able to examine those processes and achieve the same result.
1. Principle 4: The person in charge of the investigation (case officer) has overall responsibility for ensuring that the law and these principles are adhered to.

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

## Standards for Sanitizing Media
1. Russian: GOST P50739-95
    * 2 passes. Write 0; Write Random Char. No verification
1. German: VSITR
    * 7 passes. Write 0; Write 1; Write 0; Write 1; Write 0; Write 1; Write Random Char
1. American: NAVSO P-5239-26 (MFM)
    * Simpilier variation used for Modified Frequency Modulation drives such as ST506
    * 3 passes. Write 1; Write a "1" in low order bit; a "0" in the next most significant bit; and "1"s in the remaining bits comprising the data block; Writes a random char then verifies
1. American: DoD 5220.22-M
    * 3 passes. Write 0, then verify; Write 1, then verify; Write Random Char, then verify
1. American: NAVSO P-5239-26 (RLL)
    * More complex variation used for Run Length Limited Devices such as SCSI, ATA or IDE drives
    * 3 passes. Write 1; Write "0010011111..1111" (least significant bit ... most significant bit) for 32 bits and repeat this pattern throughout the data block. Repeat the appropriate pattern for all addressable data blocks.; Writes a random char then verifies
1. NIST SP 800-88
    * Clear, Purge, or Destroy


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