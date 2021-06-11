```
  162  dd if=/dev/zero of=/swapfile count=8192 bs=1M
  163  ll
  164  ls
  165  cd
  166  ls /swapfile
  167  ll swapfile
  168  pwd
  169  cd /
  170  ll swapfile
  171  vim /etc/fstab
  172  chmod 600 /swapfile
  173  mkswap /swapfile
  174  swapon /swapfile
  175  free -m
  ```
  # cat /etc/fstab
  ```
  root@foobar:/his# cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/ubuntu-vg/ubuntu-lv during curtin installation
/dev/disk/by-id/dm-uuid-LVM-cnJ85kWSe3HMDfhNcInImNBho4Kh9UA5VFlfZpy5UJXOFzo6uTiirxeYQMzs9akA / ext4 defaults 0 0
# /boot was on /dev/sda2 during curtin installation
/dev/disk/by-uuid/6543e30a-c983-43e0-920f-d2471cfd9f6b /boot ext4 defaults 0 0
# /swap.img     none    swap    sw      0       0
# /swapfile     none    swap    sw      0       0
root@foobar:/his# 
  ```
