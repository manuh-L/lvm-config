## This playbook was used to extend the swap size on the golden images. the old size was 1GB on a lvm under sda2 partition.

### Extend via ansible playbook:
<p>ex_input.yml: input values for the playbook (e.g desired size, volume group, logical...)</p>
<p>extend_swplvm.yml: the playbook to automatically extend the swap size</p>

## Extend manually
**#get new size of the disk**

`echo 1>/sys/class/block/sda/device/rescan`

**#manage partition**

`fdisk /dev/sda`

$ d -> *to delete partition number 2 (sda2)*

$ n -> *create a new Primary partition number 2 (sda2)
PRESS double ENTER (accept the default first sector, and last to use all available space)*

$ no -> *to not remove the signature of the partition*

$ t -> *change the partition type to LVM, insert **8e***

$ w -> *to save*

<p> Well done, The sda2 is now extended! </p>

**#Resize the swap lvm**

`pvresize /dev/sda2`

`swapoff -v /dev/vg_osbase/lv_swap`

`lvextend -L +7G /dev/vg_osbase/lv_swap`

`mkswap /dev/vg_osbase/lv_swap`

`swapon -v /dev/vg_osbase/lv_swap`