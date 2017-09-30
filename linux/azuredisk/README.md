## 1. create an azure disk storage class if sharehdd does not exist
#### for version >= v1.7.2
kubectl create -f https://raw.githubusercontent.com/andyzhangx/Demo/master/pv/storageclass-azuredisk.yaml
#### for version < 1.7.2
kubectl create -f https://raw.githubusercontent.com/andyzhangx/Demo/master/pv/storageclass-azuredisk-old.yaml

## 2. create an azure disk pvc
kubectl create -f https://raw.githubusercontent.com/andyzhangx/Demo/master/pv/pvc-azuredisk.yaml
#### make sure pvc is created successfully
kubectl describe pvc pvc-azuredisk

## 3. create a pod with azure disk pvc
kubectl create -f https://raw.githubusercontent.com/andyzhangx/Demo/master/linux/azuredisk/nginx-pod-azuredisk.yaml
#### watch the status of pod until its Status changed from Pending to Running
watch kubectl describe po nginx-azuredisk

## 4. enter the pod container to do validation
kubectl exec -it nginx-azuredisk -- bash

```
root@nginx-azuredisk:/# df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          30G  3.6G   26G  13% /
tmpfs           6.9G     0  6.9G   0% /dev
tmpfs           6.9G     0  6.9G   0% /sys/fs/cgroup
/dev/sda1        30G  3.6G   26G  13% /etc/hosts
/dev/sdc        4.8G   10M  4.6G   1% /mnt/blobdisk
shm              64M     0   64M   0% /dev/shm
tmpfs           6.9G   12K  6.9G   1% /run/secrets/kubernetes.io/serviceaccount
```


