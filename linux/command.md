# command

## table list
- [mount](#mount)
- [parted](#parted)
- [nmcli](#nmcli)
- [ncat](#ncat)
- [nc](#nc)
- [lsof](#lsof)
- [create big file](#create-big-file)
- [curl](#curl)
- [systemctl](#systemctl)
- [vi](#vi)
- [ps](#ps)
- [rsync](#rsync)
- [ldd](#ldd)
- [chsh](#chsh)
- [locate](#locate)
- [runuser](#runuser)

## mount
```bash=
# mount windows 路徑
mount -t cifs -o vers=2.0,user=xxx,pass=xxx,dom=domain //nas/folder /folder
```

## tar
```bash=
# 解壓到特定目錄,要先建目錄
tar -xvf file.tar.gz -C folder
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


## nmcli

觀看網卡連線狀態

```bash=
# 列出活動中的網路
nmcli connection show --active

# 列出 eth0 詳細資訊
mcli connection show eth0

# 列出所有 device
nmcli device status

# 列出 enp0s3 ipv4,ipv6 資訊
nmcli device show enp0s3

# 列出詳細關於 ipv4 的參數
nmcli connect show enp0s3|grep ipv4

```

## dnf

```bash=
dnf -y install epel-release

dnf repolist

dnf search htop

dnf group list

dnf groupinstall "Server with GUI"
```

## rpm

```bash=
# query information and file list after installed
rpm -qali|grep gzip

# 假設檔案壞掉要用套件方式把它重裝回來,用f參數查看看是哪個套件所屬
rpm -qf /etc/fstab
```

## ncat

```bash=
# -z 表示只掃描，不傳送資料
# -v 顯示詳細連線過程
# -w3 設定等待超時為 3 秒
ncat -zvw3 10.136.225.3 22
```

## nc

```bash=
# 舊
# 檢查 port 是否有 listen,0 = yes
nc -z localhost 80;echo $?
```

## ss

```bash=
# 列出特定 port 所有 connection
ss -anp | grep ":443" | grep "ESTABLISHED" | wc -l
```

## lsof

列出系統中打開的文件（包括目錄、網絡套接字等）的工具。可以用來查看系統中正在使用的文件、進程和它們之間的關係。

```bash=
#v 列出該 pid 所有使用到的關聯文件
lsof -p pid

# 列出該 account 所有開啟的文件
lsof -u account

# 列出所有 tcp process
lsof -i tcp

# 列出 22 port process
lsof -i :22

# 不要解析 dns 列出所有 tcp process
lsof -ni tcp
```

## create big file

```bash=
# create 8g file
fallocatem -l 8G /file

# create 4MB
dd if=/dev/zero of=/file bs=1M count=4096
```

## curl


-O: Saves the file with its original name
-X POST: Specifies the HTTP method
-d: Sends form data
--data-binary @file.json: Sends form file
-H: Adds a custom header to the request
-L: Follow the redirection and fetch the final content
-k: Ignores SSL certificate verification

```bash=
curl -O https://example.com/files/document.pdf

curl -u username:password -X POST -d '{"key":"value"}' -H "Content-Type: application/json" https://example.com/api

curl -X POST --data-binary @config.json -H "Content-Type: application/json" -L https://api.example.com/upload

curl -X POST -k -H "Content-Type: application/json" -d '{"query":"test"}' -L https://insecure-api.example.com/search
```

## systemctl

```bash=
# list
systemctl list-units --type=service
systemctl list-units --type=service --state=running

# 文字模式
systemctl set-default multi-user.target
or
systemctl isolate multi-user.target

/etc/inittab
```

## vi

```bash=
# 顯示行號
:set nu

# 搜尋
/keyword

# 搜尋取代全部
:%s/old/new/g

# 回第一行
:1

# 到最後一行
Shift + G
```

## ps

```bash
# 查 process 記憶體/CPU 使用率
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
```

## rsync

Rsync deamon port:873

```bash=
# A > B Sync
rsync -avhi --log-file=/temp/rsync.log source_path dest_host:/dest_path

# 透過 ssh
rsync -avhi -e ssh --log-file=/temp/rsync.log source_path dest_host:/dest_path
```

## ldd

```bash=
# 看httpd檔案的lib依賴
ldd httpd
```

## chsh

修改帳號的shell

```bash=
chsh account
```

```bash=
# 指定登入後的shell
chsh -s user1 

# 列出可用的shell
chsh -u 
```

## runuser

可以切成 root 的指令

```bash=
/usr/sbin/runuer
```