# Active Directory Home Lab

## Description
A virtual Active Directory home lab showcasing domain controller setup, user/group management, custom Group Policy testing, and Windows system administration. 

## Environment & Tools
* **Hypervisor:** VMware Workstation Pro
* **Operating Systems:** Windows Server 2022 (`DC01`), Windows 11 Enterprise (`CLIENT01`)
* **Core Technologies:**
  * Active Directory Domain Services (AD DS)
  * Group Policy Management Console (GPMC)
  * DNS Manager (Reverse Lookup Zones, PTR Records)
  * NTFS Permissions & SMB File Sharing


## Lab Walk-through

### Phase 1: Initial Server & Network Configuration
1. Installed Windows Server 2022 on a virtual machine named `DC01`.
2. Configured a permanent static IPv4 address on the server.

### Phase 2: Active Directory Installation & Structure Creation
1. Installed Active Directory Domain Services (AD DS) and DNS Server roles via Server Manager.
2. Promoted the server to Domain Controller and established a new AD forest.
3. Built a simple and scalable Organizational Unit (OU) framework with tier-1 OUs as: `Users`, `Computers`, and `Groups`, along with their respective sub-OUs.

### Phase 3: Client Deployment & Domain Join
1. Deployed a Windows 11 Enterprise virtual machine named `CLIENT01`.
2. Updated the client's IPv4 configuration to point its primary DNS directly to the static IP of `DC01`.
3. Executed an `nslookup` and `ping` from the client to verify name resolution and connection to the domain.

> [!NOTE]
> **Troubleshooting: Server Unknown**
> 
> During this step, `nslookup` returned the correct IP address but listed `Server: Unknown`. While my lab could still function, I researched the issue and discovered that I had not yet configured a Reverse Lookup Zone on my DNS server.
>
> To resolve this, I logged back into `DC01`, opened **DNS Manager**, and created a new zone under **Reverse Lookup Zones** for my network. I then turned on the **"Update associated pointer (PTR) record"** setting for both `DC01` and `CLIENT01`. The change worked immediately, and upon testing again from the client, it showed the Domain Controller's full name.

4. Joined `CLIENT01` to the Active Directory domain and then moved the client from the default Computer container into my custom sub-OU structure: `Computers` -> `Workstations`.

### Phase 4: Group Policy Implementation & Testing
1. Modified the Account Lockout parameters inside the Default Domain Policy to enforce a strict lockout policy.
2. Created a `Restrict Control Panel` GPO and linked it directly to the `Users` OU to block access for everyone in the `IT`, `Sales`, and `Marketing` sub-OUs.
3. Verified enforcement on the client machine by intentionally triggering an account lockout and attempting to open the Windows Control Panel as a standard domain user.

### Phase 5: Network File Sharing & Drive Mapping
1. Created a new folder on the `DC01` C: drive for the IT department (`\\DC01\IT`).
2. Configured the folder's advanced Security settings to grant explicit Full Control to the `IT-Admins` security group while removing access for standard users.
3. Utilized the GPMC (Group Policy Management Console) to map the network folder path as the `I:` drive.
4. Linked the drive mapping GPO to the `IT` sub-OU.

> [!NOTE]
> **Verification of Phase 5**
>
> Verified the applied security permissions by logging into `CLIENT01` under an IT admin account. The `IT (\\DC01) (I:)` drive successfully mapped to the account, allowing file creation/write permissions (tested using a text document).
>
> Upon logging into `CLIENT01` with a standard IT user account, the drive did successfully map but upon trying to access it, an access denied error message appeared, confirming that non-admin access was fully blocked.



<details><summary>📸 Click to view Phase 5 Configuration & Verification Screenshots</summary>
  <br>
  <p align="center">
    <img width="775" height="578" alt="creation_shared_drive" src="https://github.com/user-attachments/assets/28d5b89e-d03a-4d0f-af8d-f8a4755a550c" />
  </p>
</details>
