Ceph Dashboard module
=====================

## 1. Deploy Dashboard module
### 1.1 Install & enable ceph-dashboard
```bash
apt install ceph-mgr-dashboard
```

In ceph-admin
```bash
ceph mgr module enable dashboard
```

Verify
```bash
ceph mgr services
```

### 1.2 Admin user
```bash
ceph dashboard ac-user-create admin secret administrator
```

verify:
```bash
ceph dashboard ac-user-show admin
```

### 1.3 Dashboard behind Proxy server
// TODO

### 1.4 Enable HTTPS
// TODO

## 2. The Object Gateway Management frontend
```bash
radosgw-admin user create --uid=<user_id> --display-name=<display_name> --system
```

Verify:
```bash
radosgw-admin user info --uid=<user_id>
```

Finally, provide the credentials to the dashboard:
```bash
$ ceph dashboard set-rgw-api-access-key <access_key>
$ ceph dashboard set-rgw-api-secret-key <secret_key>
```

## 3. iSCSI Management
// TODO

## 4. NFS Ganesha Management
// TODO
