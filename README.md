# Claim back storage from thin provisioned disk on esxi 7 vmware
## Please give this repo a star ⭐ if you found the script useful.

## ssh login to your virtual machine
```sh
sudo -i

#extract empty blocks to a file
sudo dd if=/dev/zero of=/zerofill bs=1M
# You will see no storage available error, but it is ok, the next step will resolve this error.

#remove this file
sudo rm -f /zerofill

```
## shutdown the virtual machine 
```sh
shutdown now
```

## login to the esxi server as root

```sh
cd /vmfs/volumes/[YOUR STORAGE NAME]/[YOUR VM MACHINE NAME]

vmkfstools -K your-vmdk-file.vmdk
# until you see Hole Punching: 100% done.

# Because the new space will not always appears, the following commands will be better than restarting the whole server
/etc/init.d/hostd restart
/etc/init.d/vpxa restart
```

## boot your vm again, and check the gained space from esxi console


