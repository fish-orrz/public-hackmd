# LVM

![image](https://hackmd.io/_uploads/BJ3kn1vSa.png)


## rescan if vm extend disk space
```vash=
echo 1 > /sys/class/block/sdb/device/rescan
```

## check disk type
```bash=
df -TH

lsblk -f

blkid
```


## fdisk
```bash!
fdisk /dev/sdb
n > t,8e > w
```

## create
```bash=

pvcreate /dev/sdb1

vgcreate datavg /dev/sdb1

lvcreate -n datalv -L +50G datavg

mkfs.xfs /dev/mapper/datavg-datalv


vi /etc/fstab

/dev/mapper/datavg-datalv /data xfs default 0 0

systemctl daemon-reload

mount -a
```

## extend use lvresize
```bash!=
fdisk /dev/sdb

vgextend osvg /dev/sdb2

lvresize -L +50G /dev/mapper/osvg-varlv

xfs_growfs /dev/mapper/osvg-varlv
```

## extend use lvextend
```bash=

#check pe count
vgdisplay to check free pe count ex.12799

#free pe size
lvextend -l +12799 /dev/mapper/d2lv3

#ext2,3,4
resize2fs /dev/mapper/d2vg-d2lv3

#xfs
xfs_growfs /dev/mapper/d2vg-d2lv3

```