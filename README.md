# F5 Platform Modernization Ansible Automation

This Ansible playbook automates the modernization of F5 platforms, enabling seamless migration from older F5 BIG-IP models to newer ones. It facilitates the backup, migration, upgrade, and restoration of critical configurations, including UCS (User Configuration Set) files, crypto files, and other essential components. Additionally, this playbook supports version upgrades, such as migrating from lower versions (e.g., 15.1.x) to more recent releases (e.g., 17.5).

## PLEASE READ FIRST!

- This code is a WORK IN PROGRESS
- This code has been tested against a i10800 platform as the source and a Velos CX410 with BX110 Blade System as the Destination
- The Firmware used on the Velos Chassis Controller and Partition utilized F5OS 1.8.1 GA Code due to the Management IP
- This code hasnt been coded for rSeries or VE as of yet for the Destination 

## Prerequisites

Before using this playbook, ensure you have the following:

- Ansible Automation Platform configured in your environment.
- Access to both the source and destination F5 BIG-IP devices (both old and new).
- Backup Host (bastion) for storing the files
- SSH access to the Bastion host and F5 devices.
- The necessary F5 BIG-IP management modules installed on your Ansible control node.
- Ensure that roles for each task in the playbook are available and configured in the Ansible project directory.

## Playbook Overview

The playbook is divided into several steps, with each step performing specific tasks to ensure the migration is smooth and complete. The tasks are as follows:

### [01] Bastion Backup-Cleanup

- **Purpose**: Cleans up any previous backups on the Bastion host to ensure a fresh backup environment.
- **Hosts**: `bastion-backup`

### [02] Backup Source BIG-IP Crypto Files

- **Purpose**: Backs up the cryptographic keys and other sensitive information from the source F5 BIG-IP.
- **Hosts**: `f5_source`
- **Connection**: `local` (for local execution on the source system)

### [03] Backup Source BIG-IP UCS File

- **Purpose**: Backs up the UCS (User Configuration Set) file from the source F5 BIG-IP.
- **Hosts**: `f5_source`
- **Connection**: `httpapi` (for API connection to the source device)

### [04] Copy Source BIG-IP Crypto-UCS Files to Bastion

- **Purpose**: Copies the backed-up crypto and UCS files from the source F5 BIG-IP to the Bastion host for safekeeping.
- **Hosts**: `bastion-backup`

### [05] Disable Source F5 (BIGSTART STOP)

- **Purpose**: Disables the source F5 BIG-IP system to ensure no configurations are made while the migration process is ongoing.
- **Hosts**: `f5_source`
- **Connection**: `local`

### [06] Build and Start Destination F5

- **Purpose**: Initializes and starts the new F5 BIG-IP system (the destination device) in the F5 OS environment.
- **Hosts**: `f5_destination`
- **Connection**: `httpapi`

### [07] Setup and Restore Destination BIG-IP Crypto Files

- **Purpose**: Sets the admin password on the destination F5 BIG-IP system and restores the crypto keys.
- **Hosts**: `f5_destination`
- **Connection**: `local`

### [08] Copy Source BIG-IP UCS File to Destination BIG-IP

- **Purpose**: Copies the source F5 BIG-IP UCS file to the destination system.
- **Hosts**: `f5_destination`
- **Connection**: `httpapi`

### [09] Restore Destination BIG-IP UCS

- **Purpose**: Restores the UCS file on the destination F5 BIG-IP, replicating the configurations from the source system.
- **Hosts**: `f5_destination`
- **Connection**: `local`

### [10] Wait for UCS Restore to Complete

- **Purpose**: Waits for the UCS restore process to complete on the destination F5 BIG-IP before proceeding with further actions.
- **Hosts**: `f5_destination`
- **Connection**: `httpapi`

## Roles Used

Each step of the playbook utilizes specific roles to execute its tasks. These roles are assumed to be defined in the `roles/` directory. Ensure the following roles are available:

- `clean_up`: Cleans up backups on the Bastion host.
- `backup_crypto_keys`: Backs up crypto keys from the source F5 BIG-IP.
- `backup_info_variables`: Backs up other necessary variables and information.
- `backup_ucs_file`: Backs up the UCS file from the source system.
- `copy_files`: Manages copying of backup files.
- `disable_f5_tmm`: Disables the TMM (Traffic Management Microkernel) on the source F5 BIG-IP.
- `create_f5_os_tenant`: Initializes the destination F5 BIG-IP system.
- `start_f5_os_tenant`: Starts the F5 OS tenant on the destination system.
- `set_admin_password_shell`: Sets the admin password on the destination F5.
- `restore_crypto_keys`: Restores the crypto keys on the destination system.
- `copy_ucs_file`: Manages copying the UCS file to the destination system.
- `restore_ucs_file`: Restores the UCS file to the destination system.
- `restore_ucs_wait`: Waits for the UCS restore to complete.

## Running the Playbook in Ansible Automation Platform

### 1. **Upload Playbook to Ansible Automation Platform**

Upload the GIT Repository to the appropriate project within Ansible Automation Platform.

### 2. **Configure Inventory**

Ensure that the inventory file contains all necessary details for the source and destination F5 devices, as well as the Bastion host.

### 3. **Create a Job Template**

Create a job template in Ansible Automation Platform that specifies the following:
- Playbook to execute.
- Inventory to use.
- Any necessary credentials.

### 4. **Execute the Job Template**

Run the job template through the Ansible Automation Platform interface to automate the modernization process.

## Conclusion

This playbook offers an automated and efficient solution for modernizing F5 platforms, ensuring a seamless transition from older F5 BIG-IP models to newer ones, including version upgrades (e.g., from 15.1.x to 17.5). By leveraging Ansible Automation Platform, you can minimize downtime and manual intervention, while ensuring that critical configurations, such as UCS and crypto files, are securely and accurately transferred to the new platform.
