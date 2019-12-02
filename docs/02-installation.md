Installation
============

## 1. `ceph-admin` server
ceph-admin need `ceph` command (as well as other one) in order to perform administration tasks.

* Prerequisites
```bash
$ apt update
$ apt install curl ca-certificates apt-transport-https
```

* Add APT repository
```bash
$ curl -sSL https://download.ceph.com/keys/release.asc | apt-key add -
```

```bash
$ export CEPH_RELEASE=nautilus
$ echo "deb https://download.ceph.com/debian-${CEPH_RELEASE}/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/ceph.list
```
* Install `ceph-common`
```bash
$ apt install ceph-common cephfs-shell
```

## 2. Ceph cluster (sds servers)
* Prerequisites and APT repository configs as above

* Install Ceph packages
```bash
$ apt update
$ apt install ceph \
    ceph-mon \
    ceph-mgr \
    ceph-osd \
    ceph-mds \
    radosgw
```

* Verify version
```bash
$ ceph --version
```

## 3. `ceph-client`
* Prerequisites and APT repository configs as above

* Install `ceph-common`
```bash
$ apt install ceph-common
```
