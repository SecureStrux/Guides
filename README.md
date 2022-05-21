# Guides

## How To: Create a Tenable Nessus Scanning Account for vCenter Server Appliance (VCSA)
Performing [Nessus Credentialed Checks](https://docs.tenable.com/nessus/Content/NessusCredentialedChecks.htm) and automated [compliance audits](https://docs.tenable.com/nessus/Content/Compliance.htm) on [vCenter Server Appliance's (VCSA)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vcsa.doc/GUID-223C2821-BD98-4C7A-936B-7DBE96291BA4.html) underlying [Photon OS](https://github.com/vmware/photon) requires the creation of a privileged scanning account that defaults to the `bash` shell at logon.

### PhotonOS
PhotonOS is an open source light weight Linux distribution that is optimized for running VMware's vCenter Server. Tenable has released over 1,800 [Local Security Checks for PhotonOS](https://www.tenable.com/plugins/nessus/families/PhotonOS%20Local%20Security%20Checks).

### vCenter Server Appliance (VCSA) Roles:
VMware has established the following [VCSA Roles](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vcenter.configuration.doc/GUID-4FE047BA-5B2B-4FAB-8AC9-246ABCD3E2A1.html). The scanning account must be a **Super Administrator** for scans to succeed, as it needs access to run privileged commands from the **Bash Shell**.

#### Operator
Local users with the operator user role can read vCenter Server configuration.

#### Administrator
Local users with the administrator user role can configure vCenter Server.

#### Super Administrator
Local users with the super administrator user role can configure vCenter Server, manage the local accounts, and use the Bash shell.

### Creating the Scanning Account
To [create](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vcenter.configuration.doc/GUID-533AE852-A1F9-404E-8AC6-5D9FD65464E5.html) a privileged scanning accout:

1. Login to the vCenter Server Appliance (VCSA) using an account that has **Super Administrator** privileges. The default user with a super administrator role is **root**.
2. Once authenticated, you will be brought to the [Appliance Shell's](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vcenter.configuration.doc/GUID-393D8255-96CF-49C9-9B17-5EC639FA3DED.html) `Command>` prompt. Run the following command from the Appliance Shell to create the scanning account:
    ```Bash
    #This command will create a new user named nessus-scan. If you do not want nessus-scan to be the name of the account, change it before executing the command.
    localaccounts.user.add --username nessus-scan --role superAdmin --password
    ```
3.  When prompted, enter and then reenter a secure password.
5.  If the account is created successfully you will be brought back to the Appliance Shell's `Command>` prompt.

---
**NOTE**

It is an insecure practices to use password based authentication for highly privileged accounts. Please consider [Public Key Authentication](https://www.ssh.com/academy/ssh/public-key-authentication) using SSH keys.

---

### Change the Scanning Account's Login Shell
The default shell for new vCenter Server Appliance (VCSA) user accounts is the Appliance Shell (`/bin/appliancesh`). To perform Nessus Credentialed Checks, the scanning account's login shell must be changed from `/bin/appliancesh` to `/bin/bash`. To change the scanning account's login shell:

1. Issue the `shell` command from the Appliance Shell's `Command>` prompt to change from the **Appliance Shell** to the **Bash Shell**.
2. Set the scanning account's login shell to `/bin/bash` by executing the following command:
    ```Bash
    #This command will change the nessus-scan accounts login shell to /bin/bash. Change name of the account, if necessary.
    chsh nessus-scan --shell /bin/bash
    ```
3. Confirm the change by viewing the scanning account's login shell in `/etc/passwd` by issuing the `cat /etc/passwd` command:
   
   <img src="https://user-images.githubusercontent.com/86627856/169600276-fd2cd009-185f-479f-ba59-d7144481015e.png" width=50% height=50%></br>

### Public Key Authentication for Tenable Nessus Scanning Account
[Nessus supports DSA and RSA SSH key formats](https://docs.tenable.com/nessus/Content/SSH.htm), and Public Key Authentication is automatically enabled on vCenter Server Appliance's (VCSA) PhotonOS. Use the following steps to [create SSH keys](https://community.tenable.com/s/article/SSH-Public-Key-Authentication) for your Tenable Nessus scanning account. Complete the following steps while logged into the vCenter Server Appliance (VCSA) as the account that you created [earlier in the tutorial](https://github.com/SecureStrux/Guides/blob/main/README.md#creating-the-scanning-account).

---
**NOTE**

The following example uses the account name **nessus-scan**. Please replace **nessus-scan** with your Tenable Nessus scanning account name.

---

#### Create the Directory Structure
By default, PhotonOS stores the [public keys](https://www.ssh.com/academy/ssh/authorized-key) used to grant login access in the `.ssh/authorized_keys` file.
#### Create the Key Pair

#### Set Permissions

#### Disable Password Based Authentication
