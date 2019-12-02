Ceph Block Device (RBD)
=======================

* Admin machine
Create pools
```bash
ceph osd pool create swimmingpool 8

ceph osd pool ls
```

Associate pool to application
```bash
ceph osd pool application enable swimmingpool rbd
```

```bash
rbd pool init swimmingpool
```

Create a block device image
```bash
rbd create swimmingpool/foobar --size 1G

rbd ls
```

Create a user for client
```bash
ceph auth get-or-create client.rbd-user \
    mon 'profile rbd' \
    osd 'profile rbd pool=vms, profile rbd-read-only pool=images'
```

* Client machine

Install ceph client packages
```bash
apt update
apt install ceph-common
```

Copy `ceph.conf` and `ceph.client.rbd-user.keyring`

Map the image to a block device
```bash
rbd --name=client.rbd-user --keyring=/etc/ceph/ceph.client.rbd-user.keyring \
    map foobar -p swimmingpool
```

Format disk
```
mkfs --type=ext4 /dev/rbd/swimmingpool/foobar
```

Mount the file system
```bash
mkdir /mnt/ceph-block-device
mount /dev/rbd/swimmingpool/foobar /mnt/ceph-block-device
```

* Automatically Mapped and Mounted at boot

vi `/etc/ceph/rbdmap`:
```
# RbdDevice             Parameters
#poolname/imagename     id=client,keyring=/etc/ceph/ceph.client.keyring
swimmingpool/foobar     id=rbd-user,keyring=/etc/ceph/ceph.client.rbd-user.keyring
```

vi `/etc/fstab`
```
/dev/rbd/swimmingpool/foobar /mnt/ceph-block-device ext4 noauto 0 0
```

Enable rbdmap service:
```
systemctl enable rbdmap.service
```


Note: 
```
{image} -p {pool} == {pool}/{image}
```
