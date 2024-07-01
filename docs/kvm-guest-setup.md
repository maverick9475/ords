### Setting Up a KVM Virtual Machine with RHEL 8.10 Image

## Guide to Preparing a KVM Virtual Machine Using a QCOW2 Server Image from Red Hat

This guide will walk you through setting up a KVM virtual machine (VM) using a Red Hat Enterprise Linux (RHEL) qcow2 server image.

### Prerequisites

1. **KVM and related tools**: Ensure KVM, libvirt, and virt-manager are installed on your host system.
2. **Red Hat qcow2 Image**: Download the qcow2 image (e.g., `rhel-9.4-x86_64-kvm.qcow2`) from Red Hat's official website.

#### 1. Prepare the KVM Server Image

```bash
# Define the name of the KVM server image
KVM_SERER_IMAGE="rhel-9.4-x86_64-kvm.qcow2"

# Resize the KVM guest image to add additional space (optional)
qemu-img resize $KVM_SERER_IMAGE +10G

# Setup the root password for the new VM and remove cloud-init
virt-customize -a $KVM_SERER_IMAGE --root-password password:vagrant --uninstall cloud-init
```

- **Explanation:**
  - `qemu-img resize`: Resizes the disk image (`$KVM_SERER_IMAGE`) by adding 10GB of extra space.
  - `virt-customize`: Customizes the guest image (`$KVM_SERER_IMAGE`) by setting the root password to `vagrant` and uninstalling `cloud-init`, which is commonly used in cloud environments but not necessary for local VMs.

#### 2. Check and Set OS Variant

```bash
# Check the OS variant tag
osinfo-query os | grep -i rhel9

# Set OS variant for KVM parameters
OS_VARIANT="rhel8.10"
```

- **Explanation:**
  - `osinfo-query`: Queries the OS information database to find variants suitable for RHEL 9.
  - `OS_VARIANT`: Stores the OS variant for use in the `virt-install` command.

#### 3. Create the KVM Guest

```bash
# Create the KVM guest using virt-install
virt-install \
  --name guest1-$OS_VARIANT \
  --memory 16384 \
  --vcpus 4 \
  --disk $(pwd)/${KVM_SERER_IMAGE} \
  --import \
  --network network=default \
  --graphics none \
  --os-variant $OS_VARIANT \
  --console pty,target_type=serial
```

- **Explanation:**
  - `virt-install`: Command to create a new virtual machine.
  - Parameters:
    - `--name`: Sets the name of the VM.
    - `--memory` and `--vcpus`: Allocate memory and CPU resources.
    - `--disk`: Specifies the disk image path.
    - `--import`: Imports an existing disk image.
    - `--network`: Connects the VM to the default network.
    - `--graphics none`: Disables graphical console.
    - `--os-variant`: Specifies the OS variant for RHEL 8.10.
    - `--console`: Configures a serial console for accessing the VM.

### Post-Installation Configuration

After the VM is created, log in as root and perform the following steps:

```bash
# Register the system with Red Hat Subscription Manager
subscription-manager register
subscription-manager attach --auto

# Resize the root partition (if needed)
growpart /dev/vda 3
xfs_growfs /

# Set hostname
hostnamectl set-hostname ansibletower

# Backup and modify /etc/hosts
cp -av /etc/hosts /etc/hosts.backup.original
cp -av /etc/hosts /etc/hosts.backup.$(date +%F_%s)
cp -av /etc/hosts /etc/hosts.backup
echo "127.0.1.1 ansibletower.lan ansibletower" | tee -a /etc/hosts

# Add a new user 'vagrant' with password 'vagrant' and add to sudoers
adduser vagrant --password $(echo "vagrant" | openssl passwd -1 -stdin) -G wheel

# Display SSH login information
echo -e "\nssh vagrant@$(hostname -I)\n"

# Update the system and reboot
dnf update -y && reboot
```

### Deleting the Virtual Machine

To delete the VM:

```bash
# Destroy the VM
read DOMAIN_NAME
virsh destroy $DOMAIN_NAME

# Remove the VM and its storage
virsh undefine --remove-all-storage --nvram $DOMAIN_NAME
```

### Documentation and Resources

- Red Hat Enterprise Linux (RHEL) Extended Update Support (EUS) Overview: [Link](https://access.redhat.com/articles/rhel-eus#c8)
- How do I install the QCOW2 image provided in the RHEL downloads?: [Link](https://access.redhat.com/solutions/641193)

### Troubleshooting Tips and Recommendations

- **Networking Issues**: Ensure the VM is connected to the correct network (`default` in this case).
- **Disk Space**: Monitor disk usage to avoid running out of space after resizing.
- **Hostname and DNS**: Verify hostname and DNS settings for proper networking.
- **Red Hat Subscription**: Ensure proper registration and activation for updates and support.

