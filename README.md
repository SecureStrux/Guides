# Guides

## How To: Create a vCenter Server Appliance (VCSA) Scanning Account for Use with Tenable Nessus
Performing [Nessus Credentialed Checks](https://docs.tenable.com/nessus/Content/NessusCredentialedChecks.htm) on the vCenter Server Appliance's (VCSA) underlying [Photon OS](https://github.com/vmware/photon) requires the creation of a privileged scanning account that defaults to the `bash` shell at logon.

### vCenter Server Appliance (VCSA) Roles:
VMWare has established the following [VCSA Roles](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vcenter.configuration.doc/GUID-4FE047BA-5B2B-4FAB-8AC9-246ABCD3E2A1.html). The scanning account must be a **Super Administrator** for scans to succeed, as it needs access to run privileged commands from the **Bash Shell**.

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

### Change the Scanning Account's Login Shell
The default shell for new vCenter Server Appliance (VCSA) user accounts is the Appliance Shell (`/bin/appliancesh`). To perform Nessus Credentialed Checks, the scanning account's login shell must be changed from `/bin/appliancesh` to `/bin/bash`. To change the scanning account's login shell:

1. Issue the `shell` command from the Appliance Shell's `Command>` prompt to change from the **Appliance Shell** to the **Bash Shell**.
2. Set the scanning account's login shell to `/bin/bash` by executing the following command:
    ```Bash
    #This command will change the nessus-scan accounts login shell to /bin/bash. Change name of the account, if necessary.
    chsh nessus-scan --shell /bin/bash
    ```
3. Confirm the change by viewing the scanning account's login shell in `/etc/passwd` by issuing the `cat /etc/passwd` command
    <img src="https://user-images.githubusercontent.com/86627856/169600276-fd2cd009-185f-479f-ba59-d7144481015e.png" width=50% height=50%></br>
