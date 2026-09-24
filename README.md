# windows-server-powershell-onboarding
Automated Active Directory user provisioning and OU management system built with PowerShell and deployed in an Azure cloud lab.

# Automated Active Directory User Onboarding System

## Overview
A production-ready PowerShell automation script designed to streamline employee onboarding in an enterprise environment. Deployed and tested in a cloud-hosted Windows Server Active Directory lab environment.

## Architecture & Lab Setup
* **Cloud Infrastructure:** Microsoft Azure single VM (`Standard_D2nds_v6`) running **Windows Server 2025 Datacenter Azure Edition**.
* **Directory Services:** Active Directory Domain Services (AD DS) configured as a standalone forest (`corp.local`).
* **Management Tools:** Remote Server Administration Tools (RSAT) including Active Directory Users and Computers (ADUC) and PowerShell AD modules.

## How It Works
1. **Data Ingestion:** Reads a structured `.csv` file containing new hire details (First Name, Last Name, Department, Username).
2. **OU Management:** Automatically checks if an Organizational Unit (OU) matching the employee's department exists; if not, it creates it dynamically.
3. **User Provisioning:** Creates the user account within the correct department OU, sets a secure temporary password, and forces a password change upon first login.

## Usage & Execution
1. Clone the repository and place your `users.csv` file into the `data/` directory.


## Troubleshooting & Iterative Development

During the testing of the automated onboarding pipeline, several common PowerShell and Active Directory syntax errors were diagnosed and resolved:

### 1. Parser & Loop Syntax Errors
* **The Issue:** Encountered parser exceptions due to syntax omissions and missing whitespace in loop declarations.
  powershell
  At C:\Users\labadmin\Desktop\Deploy-Newusers.ps1:13 char:15
  + foreach ($User in$Users) {
  Missing 'in' after variable in foreach loop.
  <img width="917" height="293" alt="hit an error" src="https://github.com/user-attachments/assets/9905adec-ffd4-4bf6-bc39-92b4bc4243c8" />

The Fix: Corrected token structures and whitespace to ensure proper variable evaluation during iteration.

2. File Path Resolution (FileNotFoundException)
The Issue: Initial execution threw an open error because the script attempted to query a target dataset path with a mismatched file extension.
<img width="858" height="148" alt="a new error" src="https://github.com/user-attachments/assets/b443b40e-ab71-42cb-afdb-337979d548d3" />

The Fix: Audited local desktop environment paths and verified file asset names to establish a valid data stream.

Parameter Binding Exceptions (NamedParameterNotFound)
The Issue: Encountered parameter binding exceptions during variable path mapping and Active Directory container creation due to missing whitespace.
<img width="1197" height="617" alt="got some where" src="https://github.com/user-attachments/assets/37710a35-badc-40d0-96e1-e35b2f2950c4" />

The Fix: Enforced strict spacing syntax rules (e.g., -Path $DomainDN), allowing PowerShell to cleanly parse the parameters.

Validation & Results
Execution: Successfully processed user records through the script pipeline.

Active Directory Verification: Validated that departmental OUs and user objects were correctly provisioned under the domain structure.
<img width="1241" height="713" alt="it worked 1" src="https://github.com/user-attachments/assets/8dd101f4-bd37-44f9-97cf-863164c0a4cb" />
<img width="821" height="285" alt="it worked 2" src="https://github.com/user-attachments/assets/4211001d-96bb-4a51-a150-2124439933f8" />

Once the syntax errors and parameter bindings were resolved, the script executed successfully against the Active Directory environment (`lab.local`).

### Active Directory Verification (`dsa.msc`)

**Dynamic OU Creation:** Verified that the script automatically provisioned all target organizational units (IT, Marketing, Operations, Sales) under the domain tree.
<img width="238" height="311" alt="confirming" src="https://github.com/user-attachments/assets/475ebe0a-85ae-4a5b-85ae-b15693547a07" />


 **User Provisioning:** Confirmed that user objects (such as *John Smith* in the IT OU and *Sarah Connor* in Operations) were accurately created within their respective department containers.
 <img width="687" height="316" alt="John Smith in IT" src="https://github.com/user-attachments/assets/762dd745-9dee-4629-9a9f-51277dee81b9" />
 <img width="600" height="307" alt="Sarah Conner in operations" src="https://github.com/user-attachments/assets/1492e87d-2d71-4f35-8187-91d0f95221d7" />

 
  **Account Security Controls:** Validated that temporary credentials and account security options—such as enforcing **"User must change password at next logon"**—were populated correctly upon creation.
  <img width="492" height="370" alt="temporary passowrds were populated correctly" src="https://github.com/user-attachments/assets/037f363f-bb71-4a0f-a091-1f4be7e69b50" />
