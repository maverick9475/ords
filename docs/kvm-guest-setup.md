## Guide to Preparing a KVM Virtual Machine Using a QCOW2 Server Image from Red Hat

This guide will walk you through setting up a KVM virtual machine (VM) using a Red Hat Enterprise Linux (RHEL) qcow2 server image.

### Prerequisites

1. **KVM and related tools**: Ensure KVM, libvirt, and virt-manager are installed on your host system.
2. **Red Hat qcow2 Image**: Download the qcow2 image (e.g., `rhel-9.4-x86_64-kvm.qcow2`) from Red Hat's official website.

### Steps to Set Up the Virtual Machine

#### 1. Define the Name of the KVM Server Image

```bash
KVM_SERER_IMAGE="rhel-9.4-x86_64-kvm.qcow2"
```

#### 2. Setup the Root Password and Remove cloud-init

Use `virt-customize` to set the root password and uninstall `cloud-init`:

```bash
virt-customize -a $KVM_SERER_IMAGE --root-password password:vagrant --uninstall cloud-init
```

#### 3. Check the OS Variant Tag

Identify the OS variant tag using `osinfo-query`:

```bash
osinfo-query os | grep -i rhel9
```

#### 4. Define the OS Variant

Set the OS variant based on the output from the previous step:

```bash
OS_VARIANT="rhel9.4"
```

#### 5. Install the Virtual Machine

Use `virt-install` to create the VM with specified parameters:

```bash
virt-install \
  --name guest1-$OS_VARIANT \
  --memory 16384 \
  --vcpus 4 \
  --disk $( pwd )/${KVM_SERER_IMAGE} \
  --import \
  --network network=default \
  --graphics none \
  --os-variant $OS_VARIANT \
  --console pty,target_type=serial
```

#### 6. Login into the Server as the Root User

After the VM is created, log in as the root user and register the system:

```bash
subscription-manager register
subscription-manager attach --auto
dnf update -y && reboot
```

### Additional Documentation

For more details, refer to the following Red Hat documentation:

1. **Red Hat Enterprise Linux (RHEL) Extended Update Support (EUS) Overview**:
   [RHEL EUS Overview](https://access.redhat.com/articles/rhel-eus#c8)

2. **How to Install the QCOW2 Image Provided in the RHEL Downloads**:
   [Installing QCOW2 Image](https://access.redhat.com/solutions/641193)

By following these steps, you should have a fully functional KVM virtual machine running Red Hat Enterprise Linux.

