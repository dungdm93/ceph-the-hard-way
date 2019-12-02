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
$ gcloud compute instances create ceph-admin \
    --async \
    --image-family=ubuntu-1804-lts --image-project=ubuntu-os-cloud

# Software-Defined Storage (SDS)
$ gcloud compute instances create sds-{1..3} \
    --async \
    --source-instance-template=ceph-template

$ gcloud compute instances create ceph-client \
    --async \
    --image-family=ubuntu-1804-lts --image-project=ubuntu-os-cloud
```

### 1.3 Verification
```bash
$ gcloud compute instances list
NAME         ZONE               MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP    EXTERNAL_IP     STATUS
ceph-admin   asia-southeast1-a  n1-standard-1               10.148.15.207  35.185.176.142  RUNNING
ceph-client  asia-southeast1-a  n1-standard-1               10.148.15.211  34.87.109.138   RUNNING
sds-1        asia-southeast1-a  n1-standard-1               10.148.15.209  34.87.125.77    RUNNING
sds-2        asia-southeast1-a  n1-standard-1               10.148.15.208  34.87.26.240    RUNNING
sds-3        asia-southeast1-a  n1-standard-1               10.148.15.210  35.240.244.41   RUNNING
```
