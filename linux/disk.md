# Disk

## Extend a logical volume

```sh
sudo lvextend -L +100GiB /dev/mapper/fedora_tokamak-root
sudo xfs_growfs /
```