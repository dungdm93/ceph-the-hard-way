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

## 2. Create a Filesystem
```bash
ceph osd pool create  streaming.cephfs.data 32
ceph osd pool create  streaming.cephfs.meta 32
ceph fs new streaming streaming.cephfs.meta streaming.cephfs.data
```

> Note: Name your pools as `<application>.<fs-name>.<pool-name>` is a convention in case you have multiple Ceph applications and/or have multiple CephFSs on the same cluster.

## 3. Mounting the Filesystem
### 3.1 Prerequisites
* Generate `ceph.conf`
```bash
ceph config generate-minimal-conf -o streaming.conf
```

Then copy `streaming.conf` to the client at `/etc/ceph/ceph.conf`

* Create CephFS user
```bash
ceph fs authorize <filesystem> client.<username> {path} {permissions}

# e.g:
ceph fs authorize streaming client.cephfs-user / rw -o cephfs-user.keyring
```

Verify by `ceph auth get client.cephfs-user` command, then copy it to the client at `/etc/ceph/ceph.client.cephfs-user.keyring`

### 3.2a Mount CephFS using Kernel Driver

[ref](https://docs.ceph.com/docs/master/cephfs/kernel/#synopsis)

### 3.2b Mount CephFS using FUSE

[ref](https://docs.ceph.com/docs/master/cephfs/fuse/#synopsis)

### 3.3 Use the CephFS Shell
* Prerequisites: `cephfs-shell` is installed.

```console
root@ceph-admin:~$ cephfs-shell
CephFS:~/>>>
```

[ref](https://docs.ceph.com/docs/master/cephfs/cephfs-shell/)
