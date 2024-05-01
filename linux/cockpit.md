# 用cockpit web ui管理linux

install
```bash=
yum install cockpit
```

enable and runing
```bash=
systemctl enable --now cockpit.socket
systemctl start cockpit.socket
#default port is 9090
systemctl try-restart cockpit
```

config
```bash=
/etc/cockpit/cpckpit.conf
/etc/cockpit/disallowed-users
```

change port
```bash=
/etc/systemd/system/cockpit.socket.d/listen.conf

[Socket]
ListenStream=
ListenStream=7777
ListenStream=192.168.1.1:443
FreeBind=yes #特定IP建議設定

systemctl daemon-reload
systemctl restart cockpit.socket
```