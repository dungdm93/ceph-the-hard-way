Installation
============

## 0. Prerequisites
```bash
$ apt update
$ apt install curl ca-certificates apt-transport-https
```

## 1. Add APT repository
```bash
$ curl -sSL https://download.ceph.com/keys/release.asc | apt-key add -
```

```bash
$ export CEPH_RELEASE=nautilus
$ echo "deb https://download.ceph.com/debian-${CEPH_RELEASE}/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/ceph.list
```

## 2. Install Ceph packages
```bash
$ apt update
$ apt install ceph \
    ceph-mon \
    ceph-mgr \
    ceph-osd \
    ceph-mds \
    radosgw
```

## 3. Verify version
```bash
$ ceph --version
```
