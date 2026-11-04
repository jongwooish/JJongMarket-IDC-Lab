# 01. Basic Configuration

[한국어](../../../Session/01-basic-configuration.md) | [English](../../en/Session/01-basic-configuration.md) | [日本語](../../ja/Session/01-basic-configuration.md) | [简体中文](../../zh-CN/Session/01-basic-configuration.md) | **Español** | [Français](../../fr/Session/01-basic-configuration.md)

### [ All Devices ]

```bash
apt install -y vim ssh curl nmap rsyslog
echo -e 'set nu\nset ic\nset si' >> /etc/vim/vimrc
echo 'permitrootlogin yes' >> /etc/ssh/sshd_config && systemctl restart ssh
read hostname && echo $hostname > /etc/hostname && hostname $hostname && exit
```

En `read hostname`, introduce uno de estos: FW, DNS, WEB1, WEB2, DB, MGMT, CLIENT, REMOTE.

> Cambia también la línea `127.0.1.1` de `/etc/hosts` al nuevo nombre de host. Si la dejas igual, al reiniciar apache2 y otros servicios aparecerá un aviso de que no se puede resolver el nombre.
