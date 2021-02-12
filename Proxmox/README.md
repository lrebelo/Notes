# Proxmox Notes

## Removing a dead Node from a Cluster



## Convert a VirtualBox VM

To convert a Virtual box VM go to the location where

## Convert a XEN VM

To convert a XEN VM

## Nested virtualization

1. `cat /sys/module/kvm_intel/parameters/nested`
2. We should get a `N`
3. `echo "options kvm-intel nested=Y" > /etc/modprobe.d/kvm-intel.conf`
4. `modprobe kvm_intel`
5. `modprobe -r kvm_intel`
6. `cat /sys/module/kvm_intel/parameters/nested`
7. We should get a `Y`
8. Reboot the pve, upon reboot it should be working ok.