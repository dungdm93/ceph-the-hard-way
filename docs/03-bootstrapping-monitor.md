Bootstrapping the Ceph Monitors
===============================

## 1. Bootstraping first monitor
### 1.1 Create `/etc/ceph/ceph.conf` file
`vim /etc/ceph/ceph.conf`
```ini
[global]
fsid = 806986c7-d9b7-40f7-863e-6cc44de34f9b
mon_initial_members = ceph-1, ceph-2, ceph-3
mon_host = 10.148.15.1, 10.148.15.2, 10.148.15.3
auth_cluster_required = cephx
auth_service_required = cephx
auth_client_required = cephx
```

### 1.2 Generate an administrator keyring
Generate a `client.admin` user and add the user to the keyring.
```bash
ceph-authtool --create-keyring /etc/ceph/ceph.client.admin.keyring \
    --gen-key -n client.admin \
    --cap mon 'allow *' \
    --cap osd 'allow *' \
    --cap mds 'allow *' \
    --cap mgr 'allow *'
```

### 1.3 Generate a bootstrap-osd keyring
Generate a `client.bootstrap-osd` user and add the user to the keyring.
```bash
ceph-authtool --create-keyring /var/lib/ceph/bootstrap-osd/ceph.keyring \
    --gen-key -n client.bootstrap-osd \
    --cap mon 'profile bootstrap-osd' \
    --cap mgr 'allow r'
```

### 1.4 Generate a mon keyring
Create a keyring for your cluster and generate a monitor secret key.
```bash
ceph-authtool --create-keyring /tmp/ceph.mon.keyring \
    --gen-key -n mon. \
    --cap mon 'allow *'
```

Add the generated keys to the ceph.mon.keyring.
```bash
ceph-authtool /tmp/ceph.mon.keyring --import-keyring /etc/ceph/ceph.client.admin.keyring
ceph-authtool /tmp/ceph.mon.keyring --import-keyring /var/lib/ceph/bootstrap-osd/ceph.keyring
```

Change the ownership of ceph.mon.keyring
```bash
chown ceph:ceph /tmp/ceph.mon.keyring
```

### 1.5 Generate a monitor map
```bash
monmaptool --create --add $HOSTNAME $IP --fsid $FSID /tmp/monmap
```

In Nautilus+, create both v1 & v2:
```bash
monmaptool --create --addv $HOSTNAME [v1:$IP:6789,v2:$IP:3300] --fsid $FSID /tmp/monmap
```

* Verify monmap file by:
```bash
monmaptool --print /tmp/monmap
```

### 1.5 Populate the monitor map and keyring.
```bash
ceph-mon --mkfs -i $HOSTNAME \
    --monmap /tmp/monmap \
    --keyring /tmp/ceph.mon.keyring \
    --setuser ceph --setgroup ceph
```

> **Note**: After population, `/tmp/monmap` and `/tmp/ceph.mon.keyring` files are auto-cleaned.

### 1.6 Start the monitor
```bash
systemctl start  ceph-mon@$HOSTNAME
systemctl enable ceph-mon@$HOSTNAME
```

### 1.7 Verify monitor status
```bash
ceph --admin-daemon /var/run/ceph/ceph-mon.$HOSTNAME.asok mon_status
```
