Ceph File System (CephFS)
=========================

## 1. Deploy Metadata Server (ceph-mds)

Copy `/var/lib/ceph/bootstrap-mds/ceph.keyring`

Gen keyring
```
ceph --name client.bootstrap-mds --keyring /var/lib/ceph/bootstrap-mds/ceph.keyring \
    auth get-or-create mds.$HOSTNAME \
    osd 'allow rwx' \
    mds 'allow' \
    mon 'allow profile mds' \
    -o /var/lib/ceph/mds/ceph-$HOSTNAME/keyring
```

```
systemctl start  ceph-mds@$HOSTNAME
systemctl enable ceph-mds@$HOSTNAME
```
