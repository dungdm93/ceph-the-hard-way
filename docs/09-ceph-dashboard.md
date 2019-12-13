Ceph Dashboard module
=====================

## 1. Deploy Dashboard module
```bash
apt install ceph-mgr-dashboard
```

In ceph-admin
```bash
ceph mgr module enable dashboard

# HTTP
ceph config set mgr mgr/dashboard/ssl false

# enable TLS (HTTPS)
ceph dashboard create-self-signed-cert
...
```
