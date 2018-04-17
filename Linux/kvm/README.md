# KVM Notes

---
## Installing KVM

### Debian 8

`apt-get install qemu-kvm libvirt-bin`

### Debian 9

`apt-get install qemu-kvm libvirt-daemon-system libvirt-dev libvirt-clients`

#### Do after....

`adduser <youruser> kvm`

`adduser <youruser> libvirt`

---
## Using KVM

`virsh` command line app.

`virsh -c qemu:///system` connects us to the local domain. If we need to connect to other KVM instances we would replace the `qemu:///system` with `qemu+ssh://<SSH_USERNAME>@<IP_OF_KVM>/system`.

Once connected `list --all` to see all VMs even ones that are off.

### Exporting VMs

`virsh dumpxml <VM_NAME> > vm_name.xml` This will export the xml definition of you VM.

Then find the location of your VM from the exported xml (its under devices -> disk -> source ) and copy the file.

To Restore the VM on a new machine/server `virsh define /tmp/vm_name.xml && virsh start vm_name`
