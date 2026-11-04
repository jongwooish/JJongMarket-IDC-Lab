# 01. Basic Configuration

[한국어](../../../Session/01-basic-configuration.md) | [English](../../en/Session/01-basic-configuration.md) | [日本語](../../ja/Session/01-basic-configuration.md) | [简体中文](../../zh-CN/Session/01-basic-configuration.md) | [Español](../../es/Session/01-basic-configuration.md) | **Français**

### [ All Devices ]

```bash
apt install -y vim ssh curl nmap rsyslog
echo -e 'set nu\nset ic\nset si' >> /etc/vim/vimrc
echo 'permitrootlogin yes' >> /etc/ssh/sshd_config && systemctl restart ssh
read hostname && echo $hostname > /etc/hostname && hostname $hostname && exit
```

À `read hostname`, saisissez l'un des noms suivants : FW, DNS, WEB1, WEB2, DB, MGMT, CLIENT, REMOTE.

> Remplacez aussi la ligne `127.0.1.1` de `/etc/hosts` par le nouveau nom d'hôte. Sinon, un avertissement indiquant que le nom est introuvable s'affiche au redémarrage d'apache2 et d'autres services.
