# Disk

## Extend a logical volume

```sh
sudo lvextend -L +100GiB /dev/mapper/fedora_tokamak-root
sudo xfs_growfs /
```

## Create a logical volume

```sh
sudo vgs
# get the name of the volume group you want to add a volume to

sudo lvcreate -L 200GB -n <name> /dev/<volume-group>

sudo mkfs.xfs /dev/mapper/<volume-group>-<name>

ls -l /dev/mapper
# should show your actual disk paths

ls -l /dev/disk/by-uuid
# should show your disk again. Use this to get the uuid to add to /etc/fstab
```
