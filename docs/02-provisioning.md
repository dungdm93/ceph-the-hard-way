Provisioning Google Cloud Resources
===================================


## 1. Virtual Machines
### 1.1 Instance Template
```bash
$ gcloud compute instance-templates create ceph-template \
    --network=default \
    --machine-type=n1-standard-1 \
    --image-family=ubuntu-1804-lts --image-project=ubuntu-os-cloud \
    --create-disk=mode=rw,auto-delete=yes,size=100,type=pd-standard \
    --create-disk=mode=rw,auto-delete=yes,size=100,type=pd-standard
```

### 1.2 VM Instances
```bash
$ gcloud compute instances create ceph-{1..3} \
    --async \
    --source-instance-template=ceph-template
```

### 1.3 Verification
```bash
$ gcloud compute instances list
NAME    ZONE               MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP    EXTERNAL_IP     STATUS
ceph-1  asia-southeast1-a  n1-standard-1               10.148.15.195  34.87.26.240    RUNNING
ceph-2  asia-southeast1-a  n1-standard-1               10.148.15.197  35.185.176.142  RUNNING
ceph-3  asia-southeast1-a  n1-standard-1               10.148.15.196  35.240.244.41   RUNNING
```
