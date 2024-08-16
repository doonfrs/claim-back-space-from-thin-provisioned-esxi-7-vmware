# Claim back storage from thin provisioned disk on esxi 7 vmware

```sh
lvdisplay 

# LV Name                ubuntu-lv <=== we need this
# VG Name                ubuntu-vg <=== and this

#extract empty blocks to a file
sudo dd if=/dev/zero of=/zerofill bs=1M

#remove this file
sudo rm -f /zerofill


Now we removed all the extra space, we have to extend the file system to take all the available space, otherwise you will start to see 'no space left' error

sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv


#shutdown the os
shutdown now

#login to the esxi server as root
cd /vmfs/volumes/[YOUR STORAGE NAME]/[YOUR VM MACHINE NAME]

vmkfstools -K your-vmdk-file.vmdk

# boot your vm again, and check the gained space
```
