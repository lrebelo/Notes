# Commands shared by debian & ubuntu

* `apt-cache madison <name_of_package>` - lists all the available packet versions.

# Commands for ubuntu 18.04

## How to set static IPs on 18.04

18.04 uses netplan to set network config

`netplan generate` -> generate it!

`nano /etc/netplan/01-netcfg.yaml` -> alter it! (see next!)

```
eth0:
      dhcp4: no
      dhcp6: no
      addresses: [192.168.1.110/24, ]
      gateway4:  192.168.1.1
      nameservers:
              addresses: [8.8.8.8, 192.168.1.1]
```

`netplan apply` -> apply it!

done!
