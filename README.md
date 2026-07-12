# Active Directory Home Lab

## Description
A virtual Active Directory home lab showcasing domain controller setup, user/group management, and custom Group Policy testing.
## Environment
* **Domain Controller:** Windows Server 2022 (`DC01`)
* **Client Workstation:** Windows 11 Enterprise (`CLIENT01`)
* **Hypervisor:** VMware Workstation Pro

## Lab Walk-through

### Phase 1: Initial Server & Network Configuration
1. Installed Windows Server 2022 on a virtual machine named `DC01`.
2. Configured a permanent static IPv4 address on the server.

>[!NOTE]
>**Troubleshooting: Initial DNS Configuration broke AD Promotion**
>
>Initially I configured the server's preferred DNS server to Google's public DNS (`8.8.8.8`) to ensure the server had immediate internet access. I found out later that this broke the AD promotion and the system could not create the correct administrative folders. I researched the issue and discovered that the system was missing the `SYSVOL` and `NETLOGON` shares.
>
>To resolve this, I first changed the network settings back to point to the server's own IP address. Then, I had to go into the **Registry Editor (regedit)**, navigate to `HKLM\System\CurrentControlSet\Services\Netlogon\Parameters`, and manually change the **SysvolReady** value from `0` to `1`. This forced the system to create the folders and fixed the entire server setup.
>
>Finally, I ran the `net share` command to verify that both the folders were live and sharing over the network.  


### Phase 2: Active Directory Installation & Structure Creation
1. Installed Active Directory Domain Services (AD DS) and DNS Server roles via Server Manager.
2. Promoted the server to Domain Controller and established a new AD forest.
3. Built simple and scalable Organizational Unit (OU) framework with tier-1 OUs as: `Users`, `Computers`, and `Groups`, along with their respective sub-OUs.

### Phase 3: Client Deployment & Domain Join
1. Deployed a Windows 11 Enterprise virtual machine named `CLIENT01`.
2. Updated the client's IPv4 configuration to point its primary DNS directly to the static IP of `DC01`.
3. Executed an `nslookup` and `ping` from the client to verify name resolution and connection to the domain.

> [!NOTE]
> **Troubleshooting: Server Unknown**
> 
> During this step, `nslookup` returned the correct IP address but listed `Server: Unknown`. While my lab could still function, I researched the issue and discovered that my DNS server didn't have a way to translate IP addresses back into names.
>
> To resolve this, I logged back into `DC01`, opened **DNS Manager**, and created a new zone under **Reverse Lookup Zones** for my network. I then turned on the **"Update associated pointer (PTR) record"** setting for both the `DC01` and `CLIENT01`. The change worked immediately, and upon testing again from the client, it showed the Domain Controller's full name.

4. Joined `CLIENT01` to the Active Directory domain and then moved the client from the default Computer container into my custom sub-OU structure: `Computers` -> `Workstations`.
