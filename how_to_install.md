# Zoraxy

```
wget https://github.com/tobychui/zoraxy/releases/latest/download/zoraxy_linux_amd64
mkdir /opt/zoraxy
mv zoraxy_linux_amd64 /opt/zoraxy/zoraxy
chmod +x /opt/zoraxy/zoraxy
ln -s /opt/zoraxy/zoraxy /usr/local/bin/zoraxy
```

## Service
### For Debian based distro
```
touch /etc/systemd/system/zoraxy.service
cat <<EOF >/etc/systemd/system/zoraxy.service
[Unit]
Description=General purpose request proxy and forwarding tool
After=syslog.target network-online.target

[Service]
ExecStart=/opt/zoraxy/./zoraxy
WorkingDirectory=/opt/zoraxy/
Restart=always

[Install]
WantedBy=multi-user.target
EOF
systemctl enable -q --now zoraxy
```

### For Alpine linux

Create `/etc/init.d/zoraxy` **(chmod +x)**:

```
#!/sbin/openrc-run

description="General purpose request proxy and forwarding tool"
command="/opt/zoraxy/zoraxy"
command_background=true
directory="/opt/zoraxy"
pidfile="/run/zoraxy.pid"

depend() {
    need net
    after firewall
}
```
Maybe you must enable *fastgeoip* and cahge *port* using: `command="/opt/zoraxy/zoraxy -port=:8000 -fastgeoip=true"`.

Enable and start
```
rc-update add zoraxy default
rc-service zoraxy start
```
