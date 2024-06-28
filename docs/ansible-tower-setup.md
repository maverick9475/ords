## Guide to Install Ansible Automation Platform (Ansible Tower) on the KVM Guest Server

This guide outlines the steps to install the Ansible Automation Platform on a KVM guest server running Red Hat Enterprise Linux (RHEL) 9.4.

### Step-by-Step Installation Guide

#### 1. Initial Setup and Update

1. **Disable Bracketed Paste Mode:**
   ```bash
   bind 'set enable-bracketed-paste off'
   ```

2. **Register and Attach Subscription:**
   ```bash
   sudo subscription-manager register
   sudo subscription-manager attach --auto
   ```

3. **Update the System:**
   ```bash
   sudo dnf update -y && sudo reboot
   ```

#### 2. Configure Hostname

1. **Check Current Hostname:**
   ```bash
   hostnamectl status
   ```

2. **Set New Hostname:**
   ```bash
   sudo hostnamectl set-hostname ansibletower
   ```

3. **Backup and Modify /etc/hosts:**
   ```bash
   sudo cp -av /etc/hosts /etc/hosts.backup.original
   sudo cp -av /etc/hosts /etc/hosts.backup.$( date +%F_%s )
   sudo cp -av /etc/hosts /etc/hosts.backup
   echo "127.0.1.1 ansibletower" | sudo tee -a /etc/hosts 
   ```

4. **Update the System Again:**
   ```bash
   sudo dnf update -y && sudo dnf upgrade -y
   ```

#### 3. Install Ansible Automation Platform

1. **Verify the Setup Bundle:**
   ```bash
   ls ansible-automation-platform-setup-bundle-2.3-1.tar.gz
   ```

2. **Extract the Setup Bundle:**
   ```bash
   tar -zxf ansible-automation-platform-setup-bundle-2.3-1.tar.gz 
   cd ansible-automation-platform-setup-bundle-2.3-1
   ```

3. **Backup and Modify Inventory File:**
   ```bash
   cp -av inventory inventory.backup.original
   cp -av inventory inventory.backup.$( date +%F_%s )
   cp -av inventory inventory.backup
   ```

4. **Edit the Inventory File:**
   ```bash
   cat << EOF > inventory
   [automationcontroller]
   fqdn ansible_connection=local

   [database]

   [all:vars]
   admin_password='redhat'
   pg_host=''
   pg_port=''
   pg_database='awx'
   pg_username='awx'
   pg_password='redhat'
   EOF
   ```

5. **Run the Setup Script:**
   ```bash
   sudo ./setup.sh
   ```

#### 4. Accessing the Ansible Automation Platform Console

After the installation completes, access the Ansible Automation Platform console via the URL: `https://localhost/`. Log in with the admin username and password set in the inventory file (`admin_password='redhat'`).

#### 5. Create a Subscription Allocation

1. **Create a Subscription Allocation in the Red Hat Customer Portal:**
   - Navigate to the Red Hat Customer Portal.
   - Create a subscription allocation.
   - Export the manifest from the overview page.

2. **Upload the Manifest to Activate Your Subscription:**
   - Log in to the Ansible Automation Platform console.
   - Navigate to the subscription management section.
   - Upload the manifest file.

### Additional Documentation

For more details on installation and setup, refer to the following Red Hat documentation:

1. **How to Install Red Hat Ansible Automation Platform on RHEL 9**:
   [Red Hat Developer Article](https://developers.redhat.com/articles/2023/01/01/how-install-red-hat-ansible-automation-platform-rhel-9#)

2. **6 Steps to Install Ansible Automation Platform 2.3 on RHEL 9.1**:
   [Red Hat Developer Article](https://developers.redhat.com/articles/2023/03/07/install-ansible-23-on-rhel-91#)

3. **Raventia/ORDS Docker Image**:
   [Docker Hub](https://hub.docker.com/r/raventia/ords/tags)

By following these steps, you will have successfully installed and configured the Ansible Automation Platform on your KVM guest server.
