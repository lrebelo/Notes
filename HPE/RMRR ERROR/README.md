# RMRR ERROR when tying PCI PASSTHROUGH

__I ran this on November 2021, somethings may change for the future!__

* Flash a Mint linux (i used 20.2)
* Go to `https://downloads.linux.hpe.com/SDR/repo/stk/pool/non-free/`
  * Download the lastest version of  `hp-scripting-tools` (i used `hp-scripting-tools_11.60-20_amd64.deb`)
  * Run `dpkg -i hp-scripting-tools_11.60-20_amd64.deb` as root(or with sudo) and in the directory where it is. 
* Go to `https://downloads.linux.hpe.com/SDR/repo/mcp/pool/non-free/`
  * Download the lastest version of  `hp-health` (I used `hp-health_10.80-1874.10_amd64.deb `)
  * Run `dpkg -i hp-health_10.80-1874.10_amd64.deb` as root(or with sudo) and in the directory where it is.
* Download `https://downloads.hpe.com/pub/softlib2/software1/pubsw-linux/p1472592088/v95853/conrep_rmrds.xml`
* Create a file `exlude.dat` and save into it: `<Conrep> <Section name="RMRDS_SlotX" helptext=".">Endpoints_Excluded</Section> </Conrep>` and alter `"RMRDS_SlotX"` to the PCI slot where your card is located.
  * e.i.: The raid card I want to passthrough is located on slot1 (we can find this out in iLO) so we write: `<Conrep> <Section name="RMRDS_Slot1" helptext=".">Endpoints_Excluded</Section> </Conrep>`
* Then run! `conrep -l -x conrep_rmrds.xml -f exclude.dat` as root.

And thats all! do this for any PCI device needed (GPUs are not the only things this is usefull for :) )

**All files mentioned here are located also in the subdirectory files!** (you know... just in case HP decides to skip town with the files....) 


[updated from original](https://hardwareforever.com/2020/12/06/disable-the-rmrr-error-warning-permanently/)