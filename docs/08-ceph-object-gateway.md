Ceph Object Gateway (RGW)
=========================

## 1. Deploy RADOS Gateway (radosgw / rgw)

Copy `/var/lib/ceph/bootstrap-rgw/ceph.keyring`

Gen keyring
```
mkdir -p /var/lib/ceph/radosgw/ceph-rgw.$HOSTNAME

ceph --name client.bootstrap-rgw --keyring /var/lib/ceph/bootstrap-rgw/ceph.keyring \
    auth get-or-create client.rgw.$HOSTNAME \
    osd 'allow rwx' \
    mon 'allow rw' \
    -o /var/lib/ceph/radosgw/ceph-rgw.$HOSTNAME/keyring
```

```
systemctl start  ceph-radosgw@rgw.$HOSTNAME
systemctl enable ceph-radosgw@rgw.$HOSTNAME
```
