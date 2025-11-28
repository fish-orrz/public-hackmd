# LVM

![image](https://hackmd.io/_uploads/BJ3kn1vSa.png)


## 觸發重新 scan 磁區免重開
```vash=
echo 1 > /sys/class/block/sdb/device/rescan
```

## 常用檢查格式指令
```bash=
df -TH

lsblk -f

blkid
```

## parted
```bash=
# 要切的磁區
parted /dev/sdb
# 建 gpt table
mklabel gpt
# 建立一個名稱為 primary 分割區,
mkpart primary 0% 100%
mkpart primary 0% 20GB
# 查看有甚麼分割區
print
# 設定分割區為 lvm, 1 為 print 出來的 number,看要指定哪個做lvm
set 1 lvm on
quit
```

## fdisk
```bash!
fdisk /dev/sdb
# 新增分割區
n
# 調整分割區為 8e 
t
8e
# 存檔離開
w
```

## create
```bash=
# pv
pvcreate /dev/sdb1
# vg
vgcreate datavg /dev/sdb1
# lv
lvcreate -n datalv -L +50G datavg
# format
mkfs.xfs /dev/mapper/datavg-datalv
```

## mount
```bash=
vi /etc/fstab

/dev/mapper/datavg-datalv /data xfs default 0 0

systemctl daemon-reload

mount -a
```


## extend
```bash!=
#free pe size
lvextend -l +12799 /dev/mapper/d2lv3
lvextend -l +100%FREE /dev/mapper/d2lv3
lvresize -L +50G /dev/mapper/osvg-varlv
```

## resize
```bash=
#ext2,3,4
resize2fs /dev/mapper/d2vg-d2lv3

#xfs
xfs_growfs /dev/mapper/d2vg-d2lv3
```