# ethical-hacking-lab-practicals
An ethical hacking lab repository documenting authorized security testing using Kali Linux tools such as the Social Engineering Toolkit (SET), enum4linux, and Samba. The project demonstrates controlled attack simulations, enumeration techniques, and security assessment workflows conducted in isolated lab environments for educational purposes.
# Ethical Hacking Practice Lab (SET, enum4linux, Samba)

> **Disclaimer & Scope**
> This repository documents **authorized, educational lab exercises only**. All actions were performed in an isolated environment using intentionally vulnerable targets (e.g., DVWA / Metasploitable) with **explicit permission**. Do **not** apply these techniques to real systems without written authorization.



## Lab Overview

This lab demonstrates three common phases of an ethical hacking workflow:

1. **Social Engineering Toolkit (SET)** – Web-based credential harvesting in a controlled lab.
2. **enum4linux** – SMB/NetBIOS enumeration to identify users, shares, and domain/workgroup details.
3. **Samba (SMB)** – Share discovery and validation against a vulnerable SMB service.

**Attacker VM:** Kali Linux (VirtualBox)
**Target VMs:** DVWA, Metasploitable (lab-only)



## Tool 1: Social Engineering Toolkit (SET)

**Objective:** Demonstrate how web cloning and credential capture works in a **training environment**.

### Preconditions

* Kali Linux with SET installed.
* Target web app available **inside the lab network** (e.g., DVWA).
* Correct networking (NAT/Host-only as required).

### Procedure (Lab-Safe)

1. Launch SET with administrative privileges.
2. Navigate to **Web Attack Vectors** → **Credential Harvester**.
3. Select **Site Cloner**.
4. Configure the **POST-back IP** as the Kali host IP (lab network only).
5. Provide the **lab URL** to clone (e.g., DVWA test page).
6. Allow SET to start the local web service (HTTP/HTTPS).
7. Interact with the cloned page **from the lab client only**.
8. Observe captured POST parameters in the SET console.
9. Stop the service and export the generated report.

### Expected Output

* Console logs indicating HTTP GET/POST requests.
* A report file (XML/HTML) summarizing captured parameters.

### Key Learning Points

* How form-based credential harvesting works at a high level.
* Importance of HTTPS, input validation, and anti-phishing controls.



## Tool 2: enum4linux

**Objective:** Enumerate SMB information from a vulnerable host.

### Preconditions

* Target host exposing SMB (TCP 139/445).
* Network reachability confirmed.

### Procedure

1. Identify the target IP address.
2. Run a standard enumeration scan:

   ```bash
   enum4linux -a <TARGET_IP>
   ```
3. Review output sections:

   * Workgroup/Domain name
   * NetBIOS names
   * User listings (if permitted)
   * Share listings
4. Optionally re-run with focused flags (users, shares, OS info).

### Expected Output

* Workgroup/domain identification.
* List of available SMB shares.
* Anonymous access confirmation (if enabled in lab).

### Key Learning Points

* How misconfigured SMB services leak information.
* Value of enumeration before exploitation.



## Tool 3: Samba (SMB) Enumeration & Validation

**Objective:** Validate SMB shares and permissions discovered during enumeration.

### Preconditions

* Samba service running on the target (e.g., Metasploitable).

### Procedure

1. List shares using an SMB client:

   ```bash
   smbclient -L //<TARGET_IP> -N
   ```
2. Identify accessible shares (e.g., `tmp`, `print$`, `ADMIN$`).
3. Connect to a **non-sensitive lab share**:

   ```bash
   smbclient //<TARGET_IP>/tmp -N
   ```
4. Test basic permissions (list directory, upload/download test file).
5. Exit the session.

### Expected Output

* Successful anonymous or weak-authentication access (lab only).
* Directory listing of accessible shares.

### Key Learning Points

* Risks of anonymous SMB access.
* Importance of proper share permissions and authentication.



## Screenshots

Screenshots referenced in this repository illustrate:

* SET menu navigation and credential harvester output.
* enum4linux enumeration results.
* SMB share listings and connections.

(See `/screenshots` directory.)

---

## Defensive Recommendations

* Enforce HTTPS with HSTS.
* Deploy phishing-resistant MFA.
* Disable anonymous SMB access.
* Restrict SMB shares and apply least privilege.
* Monitor logs for abnormal POST activity.



## Legal & Ethical Notice

This documentation is for **learning and defense improvement** only. Always operate within the bounds of the law and organizational policy.


## References

* Kali Linux Documentation
* OWASP WebGoat / DVWA
* Samba Security Best Practices
