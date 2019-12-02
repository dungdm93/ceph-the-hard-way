Configuring Managers
====================

Get bootstrap-mgr keyring
```bash
ceph auth get client.bootstrap-mgr
```

Deliver to OSD host at `/var/lib/ceph/bootstrap-mgr/ceph.keyring`

If OSD host already have `ceph.client.admin.keyring`
```bash
ceph auth get client.bootstrap-mgr -o /var/lib/ceph/bootstrap-mgr/ceph.keyring
```

Generate mgr keyring
```bash
mkdir /var/lib/ceph/mgr/ceph-$HOSTNAME/

ceph --cluster ceph --name client.bootstrap-mgr --keyring /var/lib/ceph/bootstrap-mgr/ceph.keyring \
    auth get-or-create mgr.$HOSTNAME \
    mon 'allow profile mgr' \
    osd 'allow *' \
    mds 'allow *' \
    -o /var/lib/ceph/mgr/ceph-$HOSTNAME/keyring
```

Start service
```bash
systemctl start  ceph-mgr@$HOSTNAME
systemctl enable ceph-mgr@$HOSTNAME
```
