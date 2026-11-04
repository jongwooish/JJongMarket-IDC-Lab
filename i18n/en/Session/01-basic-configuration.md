# 01. Basic Configuration

[한국어](../../../Session/01-basic-configuration.md) | **English** | [日本語](../../ja/Session/01-basic-configuration.md) | [简体中文](../../zh-CN/Session/01-basic-configuration.md) | [Español](../../es/Session/01-basic-configuration.md) | [Français](../../fr/Session/01-basic-configuration.md)

### [ All Devices ]

```bash
apt install -y vim ssh curl nmap rsyslog
echo -e 'set nu\nset ic\nset si' >> /etc/vim/vimrc
echo 'permitrootlogin yes' >> /etc/ssh/sshd_config && systemctl restart ssh
read hostname && echo $hostname > /etc/hostname && hostname $hostname && exit
```

At `read hostname`, enter one of FW, DNS, WEB1, WEB2, DB, MGMT, CLIENT, REMOTE.

> Also change the `127.0.1.1` line in `/etc/hosts` to the new hostname. If you leave it, you'll get a warning that the name can't be resolved when restarting apache2 and others.
