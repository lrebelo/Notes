# HPE Smart Array P420 Controller

Run a red hat based distro (fedora live worked) or Debian based (Mint is what i used)

Then download ( https://support.hpe.com/hpesc/public/swd/detail?swItemId=MTX-9697c6899a664d02b9c3436674 ) or copy ssacli to the machine.
or for Debian based distro ( https://downloads.linux.hpe.com/SDR/repo/mcp/pool/non-free/hpssacli-2.10-14.0_amd64.deb )

Don't forget to `chmod a+x ssacli-4.17-6.0.x86_64.rpm` / `chmod a+x hpssacli-2.10-14.0_amd64.deb` for easiness.

Then `yum install ssacli-4.17-6.0.x86_64.rpm` 
or for Debian `dpkg -i hpssacli-2.10-14.0_amd64.deb`

Once installed:
(on Debian based systems go to `/opt/hp/hpssacli/bld`)
```
sudo su
ssacli
> controller slot=0 modify hbamode=on
```
or:
```
sudo ssacli controller slot=0 modify hbamode=on
```

You may need to change the slot to your card slot.

Also if you didn't delete all the logical drives you can:

```
sudo ssacli controller slot=0 delete
```
