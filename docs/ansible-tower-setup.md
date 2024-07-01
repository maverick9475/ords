Here's a detailed guide to installing Ansible Automation Platform (Ansible Tower) on your KVM guest server using the provided commands:

### Download Ansible Automation Platform from the Red Hat Website:

#### Download Red Hat Ansible Automation Platform

Make sure to use a Red Hat Developer account to access the URL below and download "Red Hat Ansible Automation Platform" version "2.3 for RHEL", or make sure the version you're downloading will RHEL OS release version you installed on your virtual machine.

https://access.redhat.com/downloads/content/480/ver=2.3/rhel---8/2.3/x86_64/product-software

Red Hat Ansible Automation Platform 2.4
https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.4


### Installing Ansible Automation Platform (Ansible Tower)

#### 1. SSH into the KVM Guest Server

```bash
ssh vagrant@<kvmguest-ip>
bind 'set enable-bracketed-paste off'
```

- **Explanation:**
  - `ssh vagrant@<kvmguest-ip>`: SSH into the KVM guest server using the IP address.
  - `bind 'set enable-bracketed-paste off'`: Disables bracketed paste mode in your terminal to avoid unintended pasting issues.

#### 2. Check sudo Privileges

```bash
sudo -l
```

- **Explanation:**
  - `sudo -l`: Checks your sudo privileges to ensure you have necessary permissions for installation steps.

#### 3. Transfer and Extract Ansible Automation Platform Setup Bundle

```bash
scp lnxuser@192.168.1.5:~/Downloads/rhel8_ansible-automation-platform-setup-bundle-2.3-5.tar.gz .
tar -zxf *.tar.gz
cd ansible*
```

- **Explanation:**
  - `scp`: Copies the setup bundle (`rhel8_ansible-automation-platform-setup-bundle-2.3-5.tar.gz`) from a remote server (`192.168.1.5`) to your current directory.
  - `tar -zxf *.tar.gz`: Extracts the setup bundle.
  - `cd ansible*`: Changes directory into the extracted Ansible Automation Platform setup directory.

#### 4. Configure Inventory File

```bash
cp -av inventory inventory.backup.original
cp -av inventory inventory.backup.$( date +%F_%s )
cp -av inventory inventory.backup

cat << EOF > inventory
[all:vars]
admin_password='<admin-user-pass>'
pg_host=''
pg_port=''
pg_database='awx'
pg_username='awx'
pg_password='<admin-user-pass>'
pg_sslmode='prefer'
automationhub_admin_password=''
automationhub_pg_host=''
automationhub_pg_port=''
automationhub_pg_database='automationhub'
automationhub_pg_username='automationhub'
automationhub_pg_password=''
automationhub_pg_sslmode='prefer'
automation_platform_version='2.3'
module_setup=True
discovered_interpreter_python='/usr/libexec/platform-python'
bundle_install='true'
setup_dir='/home/vagrant1/ansible-automation-platform-setup-bundle-2.3-5/.'
[automationcontroller]
localhost
[aap_valid_hosts]
localhost
EOF
```

- **Explanation:**
  - Creates a new `inventory` file with specified configuration settings for Ansible Automation Platform installation.
  - Sets passwords, database configurations, Ansible version requirements, and system details.

#### 5. Update Inventory and Run Setup Script

```bash
cp -av inventory inventory.backup.$( date +%F_%s )
cp -av inventory inventory.backup

sed -i -r "s/^localhost/$(hostname -I)/g" inventory

sudo ./setup.sh
```

- **Explanation:**
  - Copies and updates inventory files for backup purposes and hostname resolution.
  - Executes `./setup.sh` script with superuser privileges to initiate the Ansible Automation Platform installation.

#### 6. Access Ansible Automation Platform Console

After successful installation, access the Ansible Automation Platform console via:

- URL: `https://localhost/`
- Login: Use the admin username and password set in the inventory file.

### Troubleshooting Tips and Recommendations

- **Networking Issues**: Ensure proper network connectivity between the host and guest.
- **File Permissions**: Check and adjust file permissions as necessary for setup and configuration files.
- **Dependency Issues**: Verify all dependencies are met, especially for database connectivity and required Ansible versions.

### Documentation and Resources

How to install Red Hat Ansible Automation Platform on RHEL 9
https://developers.redhat.com/articles/2023/01/01/how-install-red-hat-ansible-automation-platform-rhel-9#

**6 steps to install Ansible Automation Platform 2.3 on RHEL 9.1
https://developers.redhat.com/articles/2023/03/07/install-ansible-23-on-rhel-91#

