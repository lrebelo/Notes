# Proxmox Notes

## Removing a dead (or soon to be) Node from a Cluster

We can get a list of nodes by running: `pvecm nodes`

We should get an output like:
```
Membership information
----------------------
Nodeid Votes Name
1 1 proxmox-node1 (local)
2 1 proxmox-node2
3 1 proxmox-node3
```

to remove a node:
* Migrate all VMs from node before the last shutdown (also any local held data)
* Shudown the node for the last time
* Wipe the OS from the node so that no "accidents" happen later. If the node comes backup after removal it will cause _problems_.
* On a live node run: `pvecm delnode proxmox-node3` 

## If we want a node to have more votes

On the server you want to have more votes change in `/etc/pve/corsync.conf` the `quorum_votes: 1` to `quorum_votes: 2`.

Beware that this only lasts untill the next reboot of the PVE server, at that stage it resets back to `quorum_votes: 1`.

## Activate IOMMU

Go to and edit `nano /etc/default/grub` and change the line `GRUB_CMDLINE_LINUX_DEFAULT=""` and add `intel_iommu=on`(intel) or `amd_iommu=on`(amd).

Then run `update-grub`. 

Then add to file `nano /etc/modules` the folowing lines:
```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

Then reboot. After that validate by running: `dmesg | grep -e DMAR -e IOMMU`

**Beware that this only works on machines with `VT-d`**

## Nested virtualization

On the PVE host make the following alterations:  
1. `cat /sys/module/kvm_intel/parameters/nested`
2. We should get a `N`
3. `echo "options kvm-intel nested=Y" > /etc/modprobe.d/kvm-intel.conf`
4. `modprobe kvm_intel`
5. `modprobe -r kvm_intel`
6. `cat /sys/module/kvm_intel/parameters/nested`
7. We should get a `Y`
8. Reboot the pve, upon reboot it should be working ok.

On the VM qemu config (ie:`/etc/pve/qemu-server/119.conf`):  
1. Make sure to make the CPU == host
2. Add in the file `args: -cpu host` (for amd `args: -cpu host,+svm`)

### HYPER-V nested virtualization is a pain...

wip
```
cpu: host
args: -cpu host,+kvm_pv_unhalt,+kvm_pv_eoi,hv_spinlocks=0x1fff,hv_vapic,hv_time,hv_reset,hv_vpindex,hv_runtime,hv_relaxed,hv_synic,hv_stimer,-hypervisor
machine: pc-i440fx-2.11
```

in certain "let good" CPUs we can not use the full code above.
```
args: -cpu SandyBridge,+kvm_pv_unhalt,+kvm_pv_eoi,hv_spinlocks=0x1fff,hv_vapic,hv_time,hv_reset,hv_vpindex,hv_runtime,hv_relaxed,hv_synic,hv_stimer,-hypervisor
```

A minimalist configuration incase of emergency (untested):
```
args: -cpu SandyBridge,hv_relaxed,hv_spinlocks=0x1fff,hv_vapic,hv_time,+vmx
```

## Convert VM hdd from other Hypervisors

### Convert a VirtualBox VM

To convert a Virtual box VM go to the location where the '.vdi' is located and copy it (with the VB VM off) to the PVE machine where we want it to go.
To do the copy I would use scp from the original location to the location on pve (ie:`/mnt/pve/pve-storage/images/100/`)

#### Convert to qcow2
`qemu-img convert -f vdi -O qcow2 virtualbox.vdi  vm-100-disk-0.qcow2`
 
#### Convert to raw
`qemu-img convert -f vdi -O raw virtualbox.vdi  vm-100-disk-0.raw`

### Convert a XEN VM

To convert a XEN VM


```
#!/bin/bash
#
# (c) 2017 GUEST.it s.r.l. - Alessandro Corbelli
# License: GPL
#
# Sample usage:
# wget --http-user=x --http-password=y http://your.xen.server.host/export?uuid=<VM_UUID> -O - | tar --to-command=./xva-conv.sh -xf -
#

TMP_LASTNAME_PREFIX="/tmp/lastname"

if [ "${TAR_FILETYPE}" != "f" ]; then
   exit
fi

if [[ "${TAR_FILENAME}" =~ "checksum" || "${TAR_FILENAME}" == "ova.xml" ]]; then
   exit
fi

DISKNAME=${TAR_FILENAME%/*}
FILENAME=${TAR_FILENAME#*/}
CURNUMBER=${FILENAME#"${FILENAME%%[!0]*}"}

# First file ? TMP_LASTNAME should be empty
if [ ! -f "${TMP_LASTNAME_PREFIX}_${DISKNAME}" ]; then
   echo ${CURNUMBER} > ${TMP_LASTNAME_PREFIX}_${DISKNAME}

   cat > ${DISKNAME}.raw 
else
   LASTNUM=$(cat ${TMP_LASTNAME_PREFIX}_${DISKNAME});

   # is sequential ?
   if [ $(expr ${LASTNUM} + 1) -eq "${FILENAME}" ]; then
      cat >> ${DISKNAME}.raw
   else
	  for i in $(seq $(expr ${LASTNUM} + 1) $(expr ${FILENAME} - 1)); do
         dd if=/dev/zero of=${DISKNAME}.raw bs=1M count=1 oflag=append conv=notrunc 2>/dev/null
      done

      cat >> ${DISKNAME}.raw
   fi

   echo ${CURNUMBER} > ${TMP_LASTNAME_PREFIX}_${DISKNAME}
fi
```
Converter bash script (copied from https://github.com/guestisp/xen-to-pve)
