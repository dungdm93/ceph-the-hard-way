Adding OSDs
===========

Get bootstrap-osd keyring
```bash
ceph auth get client.bootstrap-osd
```

Deliver to OSD host at `/var/lib/ceph/bootstrap-osd/ceph.keyring`

If OSD host already have `ceph.client.admin.keyring`
```bash
ceph auth get client.bootstrap-osd -o /var/lib/ceph/bootstrap-osd/ceph.keyring
```

Create OSD
```bash
ceph-volume lvm create --data /dev/sdb
```

Verify
```
ceph-volume lvm list
ceph status
```
