## Playbook Overview

The playbook is divided into several steps, with each step performing specific tasks to ensure a smooth and complete F5 platform modernization. The tasks are as follows:

### [01] Bastion Backup-Cleanup
- **Purpose**: Cleans up any previous backups on the Bastion host and creates a fresh backup path.  
- **Hosts**: `bastion-backup`  
- **Roles**: `clean_up`, `create_backup_path`

### [02] Backup Source BIG-IP Crypto Files
- **Purpose**: Backs up cryptographic keys and other sensitive information from the source F5 BIG-IP.  
- **Hosts**: `f5_source`  
- **Connection**: `local`  
- **Roles**: `backup_crypto_keys`, `backup_info_variables`

### [03] Backup Source BIG-IP UCS File
- **Purpose**: Backs up the UCS (User Configuration Set) file from the source F5 BIG-IP.  
- **Hosts**: `f5_source`  
- **Connection**: `httpapi`  
- **Roles**: `backup_ucs_file`

### [04] Copy Source BIG-IP Crypto-UCS Files to Bastion
- **Purpose**: Copies the backed-up crypto and UCS files from the source F5 BIG-IP to the Bastion host.  
- **Hosts**: `bastion-backup`  
- **Roles**: `copy_files_to_backup_host`

### [05] Disable Source F5 (BIGSTART STOP)
- **Purpose**: Disables the source F5 BIG-IP system to ensure no configurations are modified during migration.  
- **Hosts**: `f5_source`  
- **Connection**: `local`  
- **Roles**: `disable_f5_tmm`

### [06] Build and Start Destination F5
- **Purpose**: Initializes and starts the new F5 BIG-IP system in the F5 OS environment.  
- **Hosts**: `f5_destination_partition`  
- **Connection**: `httpapi`  
- **Roles**: `create_f5_os_tenant`, `start_f5_os_tenant`

### [07] Setup and Restore Destination BIG-IP Crypto Files
- **Purpose**: Sets the admin password on the destination F5 BIG-IP and restores crypto keys.  
- **Hosts**: `f5_destination_tenant`  
- **Connection**: `local`  
- **Roles**: `set_admin_password_shell`, `restore_crypto_keys`

### [08] Copy Source BIG-IP UCS File to Destination BIG-IP
- **Purpose**: Copies the UCS file from the source to the destination system.  
- **Hosts**: `f5_destination_tenant`  
- **Connection**: `httpapi`  
- **Roles**: `copy_ucs_file`

### [09] Restore Destination BIG-IP UCS
- **Purpose**: Restores the UCS file on the destination F5 BIG-IP, replicating the configurations from the source system.  
- **Hosts**: `f5_destination_tenant`  
- **Connection**: `ssh`  
- **Roles**: `restore_ucs`

### [10] Wait for UCS Restore to Complete
- **Purpose**: Waits for the UCS restore process to finish before any further actions.  
- **Hosts**: `f5_destination_tenant`  
- **Connection**: `httpapi`  
- **Roles**: `restore_ucs_wait`

---

## Roles Used

Each step of the playbook utilizes specific roles defined in the `roles/` directory. Ensure the following roles are available:

- `clean_up`
- `create_backup_path`
- `backup_crypto_keys`
- `backup_info_variables`
- `backup_ucs_file`
- `copy_files_to_backup_host`
- `disable_f5_tmm`
- `create_f5_os_tenant`
- `start_f5_os_tenant`
- `set_admin_password_shell`
- `restore_crypto_keys`
- `copy_ucs_file`
- `restore_ucs`
- `restore_ucs_wait`

---

## Running the Playbook in Ansible Automation Platform

### 1. **Upload Playbook**
Upload the repository to a project within Ansible Automation Platform.

### 2. **Configure Inventory**
Ensure the inventory includes all source and destination F5 devices and the Bastion host. 
check /host_vars [ bastion-backup, f5_destination_partition, f5_destination_tenant, f5_source ]

### 3. **Create a Credentials**
Create Network Credentials for Authenticating to the BIG-IP

### 4. **Create a Job Template**
Create a job template specifying:
- Playbook to execute  (standalone/migrate_standalone.yaml))
- Inventory to use 
- Necessary credentials (access to the F5 Devices, in my usecase all used the same password)
- Associate a Execution Environment that has access to F5 Collections (e.g. quay.io/f5_business_development/f5_ee_new)

### 5. **Execute the Job Template**
Run the job template through Ansible Automation Platform to automate the modernization process.

---

## Conclusion

This updated playbook offers an automated, efficient solution for modernizing F5 platforms, ensuring a seamless transition from older BIG-IP models to newer ones. By leveraging Ansible Automation Platform, downtime and manual intervention are minimized while ensuring critical configurations, such as UCS and crypto files, are securely transferred to the new platform.
