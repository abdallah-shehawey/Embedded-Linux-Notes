# Yocto Project Comprehensive Guide (Basic to Advanced)

This guide provides a comprehensive workflow for setting up the Yocto Project (Scarthgap/Kirkstone), building images, customizing layers, and performing advanced networking simulations with QEMU.

## 1. Prerequisites

Before starting, ensure your host system has the necessary dependencies installed.

**For Ubuntu/Debian:**

```bash
sudo apt-get update
sudo apt-get install gawk wget git diffstat unzip texinfo gcc build-essential \
    chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils \
    iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3 \
    xterm zstd liblz4-tool libvirt-daemon-system qemu-kvm virt-manager bridge-utils
```

**For Fedora**:

```bash
sudo dnf update -y

sudo dnf install -y \
    gawk wget git diffstat unzip texinfo gcc gcc-c++ make \
    chrpath socat cpio python3 python3-pip python3-pexpect xz \
    iputils python3-gitpython python3-jinja2 \
    mesa-libEGL SDL-devel pylint xterm zstd lz4 \
    libvirt libvirt-daemon-kvm qemu-kvm virt-manager bridge-utils
```
## 2. Setup & Building

### 2.1 Clone the Repository

Clone the Poky repository (choose your branch, e.g., `scarthgap`):

```bash
git clone -b <release-name> git://git.yoctoproject.org/poky
```

**for example** :

```bash
git clone -b scarthgap git://git.yoctoproject.org/poky
```

### 2.2 Initialize Build Environment

Navigate to the directory and source the build environment script.

```bash
cd poky
source oe-init-build-env output_of_customized_image
```

_Note: This creates a build directory named `output_of_customized_image`._

### 2.3 Build the Image

Build your target image (e.g., `core-image-minimal`) using Bitbake:

1. **Configure `local.conf`**

```bash
MACHINE ??= "qemux86-64"
DL_DIR ?= "${TOPDIR}/../downloads"
```

2. **Build the Image**

```bash
bitbake core-image-minimal
```

3. **Check Artifacts**

Built images appear in `tmp/deploy/images/<machine>/`. For example:

- `bzImage-<machine>.bin`

- `core-image-minimal-<machine>.rootfs.ext4`

## 3. Advanced Configuration and Layers

### 3.1 Add Additional Layers

To add external layers (e.g., `meta-odroid`):

1. **Clone the layer:**
    
    ```bash
    git clone https://github.com/akuster/meta-odroid
    ```
    
2. **Update `conf/bblayers.conf`:**
    
    ```bash
    BBLAYERS ?= " \
      /path/to/poky/meta \
      /path/to/poky/meta-poky \
      /path/to/poky/meta-yocto-bsp \
      /path/to/meta-odroid \
    "
    ```
    

### 3.2 Advanced `local.conf` Options

Edit `conf/local.conf` to optimize builds:

- **Speed up builds:**
    
    ```bash
    BB_NUMBER_THREADS = "8"
    PARALLEL_MAKE = "-j 8"
    ```
    
- **Enable Debug Tools:**
    
    ```bash
    EXTRA_IMAGE_FEATURES ?= "debug-tweaks tools-debug"
    ```
    

## 4. Post-Build Steps and Optimizations

1. **Check Artifacts:**
    
    - Images: `tmp/deploy/images/<machine>/`
        
    - Licenses: `tmp/deploy/licenses/`
        
2. **Cleanup:**
    
    - `bitbake -c clean <recipe>`: Removes build artifacts for one recipe.
        
    - `INHERIT += "rm_work"`: Put this in `local.conf` to remove intermediate work dirs to save disk space.
        

## 5. Running Images (QEMU)

### Standard Boot

To run the image immediately after building:

```bash
runqemu qemux86-64 nographic
```

_(Remove `nographic` if you want the graphical window)._

## 6. Saving the Kernel and Root Filesystem

To run images later without rebuilding, or to use them in the advanced networking scripts below, copy them to a safe location.

1. **Create a Directory:**
    
    ```bash
    mkdir -p ~/yocto/saved-images
    ```
    
2. **Copy Files (Example):**
    
    ```bash
    cp -L tmp/deploy/images/qemux86-64/core-image-minimal-qemux86-64.rootfs.ext4 \
       ~/yocto/saved-images/rootfs-machine1.ext4
    
    cp -L tmp/deploy/images/qemux86-64/bzImage-qemux86-64.bin \
       ~/yocto/saved-images/bzImage-machine1.bin
    ```
    

## 7. Networking Two Yocto VMs

This section sets up a **Bridge Network** allowing two QEMU instances (`node-alpha` and `node-beta`) to communicate via Static IPs.

### 7.1 Startup Script (`start_vms.sh`)

Create this script to create the bridge and launch VMs.

```bash
#!/bin/bash

# Configuration
BRIDGE_NAME="br1"
TAP1="vport11"
TAP2="vport12"

# Paths to your saved images (from Section 6)
# Ensure you have copied copies for machine1 and machine2 if you want them distinct
VM1_KERNEL=~/yocto/saved-images/bzImage-machine1.bin
VM1_ROOTFS=~/yocto/saved-images/rootfs-machine1.ext4
VM1_IP="192.168.10.2"

VM2_KERNEL=~/yocto/saved-images/bzImage-machine2.bin
VM2_ROOTFS=~/yocto/saved-images/rootfs-machine2.ext4
VM2_IP="192.168.10.3"

# 1. Create Bridge
echo "[+] Creating bridge: $BRIDGE_NAME"
sudo ip link add name $BRIDGE_NAME type bridge
sudo ip link set $BRIDGE_NAME up

# 2. Create Taps
for TAP in $TAP1 $TAP2; do
    sudo ip tuntap add mode tap $TAP
    sudo ip link set $TAP up
    sudo ip link set $TAP master $BRIDGE_NAME
done

# 3. Launch VM 1 (node-alpha)
sudo qemu-system-x86_64 \
    -kernel "$VM1_KERNEL" \
    -m 1G \
    -drive "file=$VM1_ROOTFS,if=virtio,format=raw" \
    -device virtio-net-pci,netdev=net1,mac=12:34:56:AB:CD:7B \
    -netdev tap,id=net1,ifname=$TAP1,script=no,downscript=no \
    -nographic \
    -daemonize \
    -name "node-alpha" \
    --append "root=/dev/vda rw ip=$VM1_IP::192.168.10.1:255.255.255.0::eth0:off console=ttyS0"

# 4. Launch VM 2 (node-beta)
sudo qemu-system-x86_64 \
    -kernel "$VM2_KERNEL" \
    -m 1G \
    -drive "file=$VM2_ROOTFS,if=virtio,format=raw" \
    -device virtio-net-pci,netdev=net2,mac=12:34:56:AB:CD:7C \
    -netdev tap,id=net2,ifname=$TAP2,script=no,downscript=no \
    -nographic \
    -daemonize \
    -name "node-beta" \
    --append "root=/dev/vda rw ip=$VM2_IP::192.168.10.1:255.255.255.0::eth0:off console=ttyS0"

echo "Machines launched. Use 'killall qemu-system-x86_64' to stop them."
```

### 7.2 Cleanup Script (`cleanup_vms.sh`)

Run this after stopping the VMs to remove the bridge.

```bash
#!/bin/bash
sudo ip link set dev vport11 nomaster
sudo ip tuntap del mode tap vport11
sudo ip link set dev vport12 nomaster
sudo ip tuntap del mode tap vport12
sudo ip link delete dev br1
echo "[+] Network cleanup complete."
```

### 7.3 Internal Static IP Configuration

If you prefer configuring IP inside the OS instead of kernel boot args, edit `/etc/network/interfaces` inside the running VM:

**Machine 1:**

```bash
auto eth0
iface eth0 inet static
    address 10.20.10.2
    netmask 255.255.255.0
    gateway 10.20.10.1
```

## 8. Troubleshooting and Tips

- **QEMU Permissions:** If you get "Operation Not Permitted", ensure your user has rights to the tap devices or run with `sudo`.
    
- **Networking:** If VMs can't ping each other, check if the firewall (ufw/iptables) on the host is blocking the bridge `br1`.
    
- **Performance:** Add `kvm` to the qemu command (e.g., `runqemu qemux86-64 kvm`) if your host supports virtualization.
    
- **Debugging:** Use `bitbake -c devshell <recipe>` to enter a development shell for a specific package.
    

## Resources

- [Yocto Project Documentation](https://docs.yoctoproject.org/index.html "null")
    
- [OpenEmbedded](https://www.openembedded.org/ "null")